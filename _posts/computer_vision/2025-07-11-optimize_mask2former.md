---
layout: post
title: Optimize model for TensorRT to infer
category: Computer Vision
tag: [trt, tensorrt]
---


## 문제

파이선 모델 (`pt`, `pth`, `h5` 등)을 **TensorRT**로 변환해서 inference를 하면 당연히 속도는 빨라짐

***하지만, 배치가 늘어남에 따라 선형은 아니더라도 inference 속도가 줄어듬***

    * 샘플당 inference 시간은 줄지만, 원하는 배치만큼의 시간은 배치가 1일때보다 오래 걸림
    * 배치마다 병렬로 동작할 것이라는 판단하에 배치가 늘어도 inference시간은 1과 비슷할 것이라 예상하였으나 아니었음
    * 이는 GPU 리소스를 각 배치마다 inference할 때, 독립적으로 사용하는 것이 아니라 공유하기 때문이라고 추측
        * 즉, 완전한 병렬이 될 수 없음



## 해결법

1. **CUDA Stream**을 배치만큼 독립적으로 정의하여 배치마다 병렬로 동작할 수 있도록 하기

    ```cpp
    void runInference(IExecutionContext* context, void* buffers[], cudaStream_t stream) {
        if (!context->enqueueV2(buffers, stream, nullptr)) {
            std::cerr << "Inference failed." << std::endl;
        }
    }

    int main() {
        // 엔진과 context 준비
        ICudaEngine* engine = ...;
        IExecutionContext* context1 = engine->createExecutionContext();
        IExecutionContext* context2 = engine->createExecutionContext();

        // stream 생성
        cudaStream_t stream1, stream2;
        cudaStreamCreate(&stream1);
        cudaStreamCreate(&stream2);

        // 입력/출력 버퍼 준비 (배치1용)
        void* buffers1[2]; // input/output
        void* buffers2[2]; // input/output
        // 각 버퍼에 대한 cudaMalloc 및 데이터 복사 생략

        // 병렬 실행
        std::thread t1(runInference, context1, buffers1, stream1);
        std::thread t2(runInference, context2, buffers2, stream2);

        t1.join();
        t2.join();

        cudaStreamSynchronize(stream1);
        cudaStreamSynchronize(stream2);

        // 리소스 해제 생략
        return 0;
    }
    ```

2. **Optimization Profile** 설정 (Dynaymic Batch)

    ```cpp
    IBuilder* builder = createInferBuilder(logger);
    INetworkDefinition* network = builder->createNetworkV2(0);
    IBuilderConfig* config = builder->createBuilderConfig();

    // 입력 정의
    ITensor* input = network->addInput("input", DataType::kFLOAT, Dims4{-1, 3, 224, 224});

    // Optimization Profile 설정
    IOptimizationProfile* profile = builder->createOptimizationProfile();
    profile->setDimensions("input", OptProfileSelector::kMIN, Dims4{1, 3, 224, 224});
    profile->setDimensions("input", OptProfileSelector::kOPT, Dims4{4, 3, 224, 224});
    profile->setDimensions("input", OptProfileSelector::kMAX, Dims4{8, 3, 224, 224});
    config->addOptimizationProfile(profile);

    // 이후 builder->buildEngineWithConfig(network, config) 실행

    ```

    * Dynamic batch 사용 시 context->setBindingDimensions()을 반드시 호출해야 합니다.
        ```cpp
        context->setBindingDimensions(0, Dims4{4, 3, 224, 224});
        ```

3. TensorRT에서 Layer Fusion 확인 및 디버깅

* `--dumpLayerInfo` or `--dumpProfile`

    ```bash
    trtexec --onnx=model.onnx --fp16 --dumpProfile
    ```
        
    * "Layer fusion이 되었다면" Layer 수가 원래보다 훨씬 줄어듦
    * Time %가 많은 layer가 fused되지 않은 핵심 병목 layer임

* `--exportProfile`로 JSON으로 저장 가능

    ```bash
    trtexec --onnx=model.onnx --exportProfile=profile.json
    ```
    * 추후 자동 분석 도구에서 시각화 가능


#### Layer Fusion

* "연산적으로 연속되는 연산들(conv → bias → activation 등)을 한 커널로 합쳐서, GPU memory access와 kernel launch overhead를 줄이고 속도를 높이는 최적화 기법"

* **TensorRT**는 **ONNX** 또는 **TensorFlow** 등의 모델을 `import`할 때 자동으로 **layer fusion**을 시도합니다. 


* Layer Fusion을 유도하기 위한 모델 작성 규칙 (PyTorch 기준)
    | 연산 구조                | 권장 작성 방식                                            |
    | -------------------- | --------------------------------------------------- |
    | Conv + BN + ReLU     | `nn.Sequential(nn.Conv2d, nn.BatchNorm2d, nn.ReLU)` |
    | Conv + Sigmoid       | `nn.Sequential(...)` 로 묶어서 ONNX에 잘 나타나게             |
    | Layer 내부 계산          | `x = self.act(self.conv(x))` → 대신 block으로 묶기        |
    | Depthwise + BN + Act | DepthwiseBlock 클래스로 통합                              |




#### 조건들

* 조건1: 연속된 layer들이 단일 연산 블록으로 합쳐질 수 있어야 함

    * Conv → BiasAdd → ReLU → ✅ 가능
    * Conv → Reshape → ReLU → ❌ Reshape가 막음

* 조건2: 연산 사이에 shape 변경, cast, split, identity 등이 없어야 함

    * Conv → Sigmoid → Multiply ← 중간의 Sigmoid나 Mul은 fusion 막을 수 있음

* 조건 3: 연산들이 statically computable해야 함
    
    * dynamic shape이 많거나 conditional branch가 있을 경우, fusion 실패

* 조건 4: custom plugin이 없거나, fusion 지원 plugin이어야 함
    
    * custom op가 있는 경우, fusion 대상에서 제외됨



#### Layer Fusion 실패하는 주요 원인과 대응

* 이유 1: ONNX 내에 Identity, Reshape, Cast, Transpose 등 불필요한 중간 연산 존재

    * 대응:
        
        * ONNX export 시 unnecessary node를 제거

            ```python
            torch.onnx.export(..., do_constant_folding=True)
            ```

    * onnxsim으로 단순화:

        ```python
        onnxsim model.onnx model_simplified.onnx
        ```

    * onnxoptimizer로 redundant ops 제거:

        ```python
        from onnxoptimizer import optimize
        model = onnx.load("model.onnx")
        optimized_model = optimize(model, passes=["eliminate_identity", "eliminate_nop_transpose"])
        ```



* 이유 2: BatchNorm이 Conv와 분리된 layer로 존재

    * 대응:

        * PyTorch에서는 model.eval() 상태에서 export할 것
        * 그래야 BN이 scale/shift로 바뀌어 Conv에 흡수됨

        * 또는 BN을 직접 fuse
            ```python
            from torchvision.models.quantization.utils import fuse_modules
            model = fuse_modules(model, [["conv", "bn", "relu"]])
            ```
* 이유 3: Conv, Activation, ElementWise가 서로 다른 subgraph로 분리됨

    * 대응:

        * Conv + ReLU, Conv + Sigmoid 등의 activation을 함수형으로 쓰지 말고 모듈형으로 작성

        ```python
        # ❌ 나쁜 예
        x = F.relu(self.conv(x))

        # ✅ 좋은 예
        self.block = nn.Sequential(
            nn.Conv2d(...),
            nn.ReLU(inplace=True)
        )
        ```

* 이유 4: 중간에 Constant, Unsqueeze, Cast, Pad 등의 얕은 연산이 끼어 있는 경우
    
    * 대응:

        * `torch.onnx.export(..., operator_export_type=ONNX_FALLTHROUGH)`로 조절하거나

        * 해당 연산이 실제로 필요한지 점검 후 제거

        * `onnx-graphsurgeon`을 통해 수동 수정도 가능


#### TensorRT에서 수동 Layer Fusion 유도법 (고급)

* 방법 1: ONNX Graph Surgeon 사용 (onnx-graphsurgeon)
    ```python
    import onnx_graphsurgeon as gs
    import onnx

    graph = gs.import_onnx(onnx.load("model.onnx"))
    # 예: Identity 제거
    graph.cleanup().toposort()
    onnx.save(gs.export_onnx(graph), "cleaned_model.onnx")
    ```

* 방법 2: TensorRT Graph Viewer (Polygraphy)

    TensorRT의 내부 fusing이 어떤 식으로 일어났는지 시각화 가능
    ```bash
        polygraphy surgeon inspect model.onnx --show layers
    ```


#### 체크리스트

| 체크 항목                                               | OK 기준 |
| --------------------------------------------------- | ----- |
| `model.eval()` 상태에서 ONNX export 했는가?                | ✅     |
| 불필요한 연산 (`Cast`, `Reshape`, `Identity`) 제거했는가?      | ✅     |
| activation을 module 형태로 선언했는가?                       | ✅     |
| Conv-BN-Activation을 Sequential 또는 하나의 block으로 묶었는가? | ✅     |
| `onnxsim`, `onnxoptimizer` 사용했는가?                   | ✅     |
| FP16/INT8 inference를 시도했는가?                         | ✅     |
| TensorRT profile에서 가장 느린 layer들이 fusion 불가능한 구조인가?  | ✅     |
| 엔진 빌드 시 workspace를 충분히 줬는가? (`--workspace=4096` 이상) | ✅     |

#### 결론: Layer Fusion 완전 대응 전략

* ONNX 최적화 파이프라인 (PyTorch 기준)

    1. model.eval() 로 설정
    2. ONNX export (do_constant_folding=True, dynamic_axes)
    3. onnxsim 으로 단순화
    4. onnxoptimizer 로 경량화
    5. onnx_graphsurgeon 으로 불필요한 노드 제거
    6. trtexec 로 engine 생성하며 --dumpProfile 확인
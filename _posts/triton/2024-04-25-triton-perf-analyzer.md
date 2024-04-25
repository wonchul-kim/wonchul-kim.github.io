---
layout: post
title: Performance Analyzer
category: Triton
tag: ['triton', 'perf analyzer']
---

## Triton Server Container

#### Prepare the `model_repository` 

| model_repository 
        | -------- name01
                    | -------- 1
                               | --------- model.onnx 
                    | -------- config.pbtxt
                    | -------- output_labels.txt
                   
        | -------- name02
                    | -------- 1
                               | --------- model.onnx 
                    | -------- config.pbtxt
                    | -------- output_labels.txt
         

#### Run the triton server container
```
docker run --gpus all --rm -it --net host -v <local model-respoitory>:<container model-repository> nvcr.io/nvidia/tritonserver:${RELEASE}-py3
```

#### Run the triton server inside the container
```
tritonserver --model-repository <container model-repository> &> server.log &
```

For example, 
```
tritonserver --model-repository $(pwd)/model_repository &> server.log &
```

#### Check if the triton server works
Outside of the triton server container, 
```
curl -v localhost:8000/v2/health/ready
```

If it works well, you must see the below:
<img src='/assets/triton/curl_triton_server.png'>


## Triton Inference Server SDK

The model weights and configurations are in **triton server container**, not in **triton inference server SDK container**.


#### Run the triton sdk container
```
docker run -it --gpus all -v /var/run/docker.sock:/var/run/docker.sock -v /HDD/_projects/etc/triton:/triton --net=host nvcr.io/nvidia/tritonserver:23.07-py3-sdk
```

The `$workspace` will look like the below:
<img src='/assets/triton/sdk_workspace.png'>


#### Run the perf analyzer

Inside the triton sdk container,
``````

```
perf_analyzer -m simple
```

Then, the below will be shown:
<img src='/assets/triton/perf_analyer_simple.png'>

> The `simple` model does not exist at triton sdk container, it does at `model-repository` in triton server container.

## References
- [Triton Performance Analyzer](https://github.com/triton-inference-server/client/blob/main/src/c++/perf_analyzer/README.md)

- https://velog.io/@hbjs97/triton-inference-server-%EC%84%B1%EB%8A%A5-%EC%B5%9C%EC%A0%81%ED%99%94
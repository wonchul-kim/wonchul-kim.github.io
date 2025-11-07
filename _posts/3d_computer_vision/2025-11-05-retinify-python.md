---
layout: post
title: Retinify for python
category: 3D Computer Vision
tag: [retinify, stereo matching]
---

# [Retinify](https://retinify.ai/)

**Retinify**는 딥러닝 모델은 TensorRT를 지원하는 inference이지만, C++에서 사용할 수 있는 라이브러리로 지원을 해준다. 

그렇기 때문에 Python에서 곧바로 사용할 수는 없어서 `pybind` 또는 `ctypes`를 활용해서 Python에서도 사용할 수 있도록 할 수 있다.

### 준비물

#### 1. `retinify_wrapper.hpp`

```cpp
// retinify_wrapper.hpp

#ifndef RETINIFY_WRAPPER_HPP
#define RETINIFY_WRAPPER_HPP

#include <opencv2/opencv.hpp>
#include <retinify/retinify.hpp>
#include <string>

// getDepth_wrapper 함수의 선언
cv::Mat getDepth_wrapper(const cv::Mat& leftImage, const cv::Mat& rightImage);

#endif // RETINIFY_WRAPPER_HPP
```

#### 2. `retinify_wrapper.cpp`

```cpp
#include <iostream>
#include "retinify_wrapper.hpp"

// Pybind11 바인딩을 위한 헬퍼 함수
cv::Mat getDepth_wrapper(const cv::Mat& leftImage, const cv::Mat& rightImage)
{

    std::cout << "Left Image Channels: " << leftImage.channels() 
            << ", Stride: " << leftImage.step[0] 
            << ", Expected Stride (Approx): " << leftImage.cols * leftImage.elemSize() << std::endl;
    retinify::Pipeline pipeline;
    
    if (leftImage.empty() || rightImage.empty()) {
        throw std::runtime_error("Failed to load input images.");
    }

    cv::Mat disparity = cv::Mat::zeros(leftImage.size(), CV_32FC1);
    
    // 1. 초기화
    auto statusInitialize = pipeline.Initialize(static_cast<std::uint32_t>(leftImage.cols), static_cast<std::uint32_t>(leftImage.rows));
    if (!statusInitialize.IsOK()) {
        // throw std::runtime_error("Failed to initialize the pipeline: " + statusInitialize.GetErrorMessage());
        throw std::runtime_error("Initialize failed (retinify::Status not OK).");
    }
    else std::cout << "\nSuccessfully Initialized Retinify Pipeline";

    // 2. 실행
    auto statusExecute = pipeline.Execute(leftImage.ptr<uint8_t>(), leftImage.step[0], rightImage.ptr<uint8_t>(), rightImage.step[0]);
    if (!statusExecute.IsOK()) {
        // throw std::runtime_error("Failed to execute the pipeline: " + statusExecute.GetErrorMessage());
        throw std::runtime_error("Execute failed (retinify::Status not OK).");
    }
    else std::cout << "\nSuccessfully Executed Retinify Pipeline";

    // 3. 결과 가져오기 (Disparity Map)
    auto statusRetrieve = pipeline.RetrieveDisparity(disparity.ptr<float>(), disparity.step[0]);
    if (!statusRetrieve.IsOK()) {
        // throw std::runtime_error("Failed to retrieve the disparity map: " + statusRetrieve.GetErrorMessage());
        throw std::runtime_error("RetrieveDisparity failed (retinify::Status not OK).");
    }
    else std::cout << "\nSuccessfully Retrived Disparity from Retinify Pipeline";

    return disparity;
}
```

#### 3. `retinify_binding.cpp`
```cpp
#include <pybind11/pybind11.h>
#include <pybind11/numpy.h>
#include "retinify_wrapper.hpp"
#include "opencv_numpy_conversions.h" // Mat <-> NumPy 변환 헬퍼 파일

namespace py = pybind11;

// retinify_binding.cpp 파일 내
PYBIND11_MODULE(retinify_py, m) {
    m.doc() = "Retinify depth estimation module.";

    m.def("get_depth",
        [](const py::array left_py, const py::array right_py) -> py::array {
            cv::Mat left  = array_to_mat(left_py);
            cv::Mat right = array_to_mat(right_py);

            // 1) 채널 정규화: GRAY → BGR
            if (left.channels() == 1)  cv::cvtColor(left,  left,  cv::COLOR_GRAY2BGR);
            if (right.channels() == 1) cv::cvtColor(right, right, cv::COLOR_GRAY2BGR);

            // 2) 깊이 정규화: 16U/32F → 8U
            if (left.depth() != CV_8U) {
                double alpha = (left.depth() == CV_16U) ? (1.0/256.0) : 1.0; // 대략 스케일
                left.convertTo(left, CV_8U, alpha);
            }
            if (right.depth() != CV_8U) {
                double alpha = (right.depth() == CV_16U) ? (1.0/256.0) : 1.0;
                right.convertTo(right, CV_8U, alpha);
            }

            // 3) 연속 메모리 보장 (stride = cols*3)
            if (!left.isContinuous())  left  = left.clone();
            if (!right.isContinuous()) right = right.clone();

            cv::Mat result = getDepth_wrapper(left, right);
            return mat_to_array<float>(result);
        }, py::arg("left_image_array"), py::arg("right_image_array"),
        "Computes colored disparity map from image paths and returns it as a NumPy array (BGR, uint8)."
        
    );

    m.def("get_disparity_strict",
    [](const py::array left_py, const py::array right_py) -> py::array {
        cv::Mat left  = array_to_mat(left_py);
        cv::Mat right = array_to_mat(right_py);

        // 1) CV_8UC3 강제
        auto to_8uc3 = [](cv::Mat& img) {
            if (img.type() == CV_8UC3) return;

            // depth 보정
            if (img.depth() != CV_8U) {
                cv::Mat tmp;
                img.convertTo(tmp, CV_8U);
                img = std::move(tmp);
            }

            // 채널 보정
            if (img.channels() == 1) {
                cv::cvtColor(img, img, cv::COLOR_GRAY2BGR);
            } else if (img.channels() == 4) {
                cv::cvtColor(img, img, cv::COLOR_BGRA2BGR);
            } else if (img.channels() != 3) {
                throw std::runtime_error("Unsupported channels for input image");
            }
        };

        to_8uc3(left);
        to_8uc3(right);

        // 2) 연속(contiguous) 보장
        if (!left.isContinuous())  left  = left.clone();
        if (!right.isContinuous()) right = right.clone();

        // 디버깅: 기대 stride = cols * 3
        std::cout << "Left ch=" << left.channels()
                << " step=" << left.step[0]
                << " expected=" << (left.cols * 3) << "\n";

        // === Retinify ===
        retinify::Pipeline pipe;
        auto s0 = pipe.Initialize((uint32_t)left.cols, (uint32_t)left.rows);
        if (!s0.IsOK()) throw std::runtime_error("Initialize failed");

        auto s1 = pipe.Execute(
            left.ptr<uint8_t>(),  left.step[0],
            right.ptr<uint8_t>(), right.step[0]);
        if (!s1.IsOK()) throw std::runtime_error("Execute failed");

        cv::Mat disp(left.size(), CV_32FC1);
        auto s2 = pipe.RetrieveDisparity(disp.ptr<float>(), disp.step[0]);
        if (!s2.IsOK()) throw std::runtime_error("RetrieveDisparity failed");

        return mat_to_array<float>(disp);  // float32 (H,W)
    });


    m.def("colorize_native",
        [](const py::array disp_py, float scale) -> py::array {
            // NumPy(float32, HxW) → Mat
            cv::Mat disp = array_to_mat(disp_py);
            if (disp.type() != CV_32FC1) {
                // 안전: float32 단일 채널로 보정
                disp.convertTo(disp, CV_32F);
                if (disp.channels() != 1) {
                    std::vector<cv::Mat> chs; cv::split(disp, chs);
                    disp = chs[0]; // 첫 채널만 사용 (원하는 규칙으로 바꿔도 됨)
                }
            }

            // 출력 버퍼 (Retinify는 RGB 순서로 채움)
            cv::Mat colored(disp.size(), CV_8UC3);
            auto st = retinify::ColorizeDisparity(
                disp.ptr<float>(),          disp.step[0],
                colored.ptr<uint8_t>(),     colored.step[0],
                disp.cols, disp.rows, scale
            );
            if (!st.IsOK()) {
                throw std::runtime_error("ColorizeDisparity failed (Status not OK).");
            }

            // OpenCV 저장/표시 호환을 위해 RGB→BGR 변환
            cv::cvtColor(colored, colored, cv::COLOR_RGB2BGR);

            // Mat → NumPy(uint8, HxWx3)
            return mat_to_array<uint8_t>(colored);
        }, py::arg("disp_f32"), py::arg("scale") = 256.0f,
        "Apply Retinify's native ColorizeDisparity (identical coloring)."
    );
}
```

#### 4. `opencv_numpy_conversions.h`
```cpp
#ifndef OPENCV_NUMPY_CONVERSIONS_H
#define OPENCV_NUMPY_CONVERSIONS_H

#include <pybind11/pybind11.h>
#include <pybind11/numpy.h>
#include <opencv2/opencv.hpp>
#include <stdexcept>
#include <cstring>   // std::memcpy

namespace py = pybind11;

// =================================================================
// 1) NumPy Array -> OpenCV Mat (copy-once: 안전하게 clone)
//    - dtype 크기(itemsize)로 depth 결정
//    - CV_MAKETYPE(depth, channels)로 정확히 타입 조합
//    - NumPy의 stride[0]을 step으로 넣고, 끝에 clone()으로 연속 메모리 보장
// =================================================================
inline cv::Mat array_to_mat(py::array arr) {
    if (arr.ndim() > 3) {
        throw std::runtime_error("NumPy array dimensions must be 1, 2, or 3.");
    }

    const int channels = (arr.ndim() == 3) ? static_cast<int>(arr.shape()[2]) : 1;

    // depth 결정
    const size_t item = arr.itemsize();
    int depth = -1;
    if      (item == 1) depth = CV_8U;
    else if (item == 2) depth = CV_16U;
    else if (item == 4) depth = CV_32F;
    else if (item == 8) depth = CV_64F;
    else throw std::runtime_error("Unsupported NumPy dtype (uint8/uint16/float32/float64 only).");

    const int cv_type = CV_MAKETYPE(depth, channels);

    py::buffer_info info = arr.request();
    const int rows = static_cast<int>(arr.shape()[0]);
    const int cols = (arr.ndim() >= 2) ? static_cast<int>(arr.shape()[1]) : 1;

    // NumPy 메모리를 step 포함해 cv::Mat로 감싸고, 소유권 분리 위해 clone()
    // (info.strides[]는 바이트 단위)
    cv::Mat m(rows, cols, cv_type, info.ptr,
              (info.ndim >= 1 ? static_cast<size_t>(info.strides[0]) : 0));
    return m.clone(); // 항상 연속(contiguous) 보장
}

// =================================================================
// 2) OpenCV Mat -> NumPy Array (항상 복사: UAF/크래시 방지)
// =================================================================
template <typename T>
inline py::array_t<T> mat_to_array(const cv::Mat& mat) {
    cv::Mat src = mat.isContinuous() ? mat : mat.clone();

    // (선택) 템플릿 T와 Mat depth 간 간단한 안전 체크
    // 필요 없다면 이 블록은 제거해도 됩니다.
    // - T=uint8_t  -> depth=CV_8U
    // - T=float    -> depth=CV_32F
    // - T=double   -> depth=CV_64F
    // - T=uint16_t -> depth=CV_16U
    {
        int expected = -1;
        if      (std::is_same<T, uint8_t>::value)  expected = CV_8U;
        else if (std::is_same<T, uint16_t>::value) expected = CV_16U;
        else if (std::is_same<T, float>::value)    expected = CV_32F;
        else if (std::is_same<T, double>::value)   expected = CV_64F;
        if (expected != -1 && src.depth() != expected) {
            throw std::runtime_error("mat_to_array<T>: cv::Mat depth and template T mismatch.");
        }
    }

    std::vector<size_t> shape = (src.channels() > 1)
        ? std::vector<size_t>{ static_cast<size_t>(src.rows),
                               static_cast<size_t>(src.cols),
                               static_cast<size_t>(src.channels()) }
        : std::vector<size_t>{ static_cast<size_t>(src.rows),
                               static_cast<size_t>(src.cols) };

    py::array_t<T> out(shape);
    std::memcpy(out.mutable_data(), src.data, src.total() * src.elemSize());
    return out;  // 파이썬이 메모리 소유
}

#endif // OPENCV_NUMPY_CONVERSIONS_H
```

#### 5. `CMakeLists.txt`
```cmake
# C++ 표준을 20으로 설정
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# =================================================================
# ⭐ 1. Pybind11 모듈 빌드 설정 추가 ⭐
# =================================================================

# BUILD_PYTHON_BINDINGS 옵션을 추가하여 빌드를 제어합니다.
option(BUILD_PYTHON_BINDINGS "Build Python bindings using pybind11" ON)

if(BUILD_PYTHON_BINDINGS)
    message(STATUS "Building Python Bindings (retinify_py)")

    # ⭐⭐ [1단계] OpenCV 패키지 찾기 추가 ⭐⭐
    # OpenCV를 찾습니다. 이전 단계에서 -DOpenCV_DIR을 사용했으므로 성공해야 합니다.
    find_package(OpenCV REQUIRED) 
    
    # pybind11 패키지 찾기
    find_package(pybind11 REQUIRED)
    
    # NumPy 지원을 위해 numpy 헤더 경로 추가
    find_package(Python COMPONENTS Development)
    
    # 'retinify_py'라는 이름의 파이썬 모듈을 정의합니다.
    pybind11_add_module(retinify_py SHARED
        retinify_binding.cpp
        retinify_wrapper.cpp
    )
    
    # ❗ OpenCV 라이브러리와 retinify C++ 라이브러리에 링크해야 합니다.
    target_link_libraries(retinify_py PRIVATE
        retinify             # retinify의 핵심 C++ 라이브러리
        ${OpenCV_LIBS}       # OpenCV
    )

    # Pybind11/NumPy 헤더 포함
    target_include_directories(retinify_py PRIVATE
        ${pybind11_INCLUDE_DIRS}
        ${PYTHON_INCLUDE_DIRS}
        # ⭐⭐ [2단계] OpenCV Include 경로 추가 ⭐⭐
        ${OpenCV_INCLUDE_DIRS} 
    )

    # ❗ 모듈이 찾을 수 있도록 런타임 경로에 빌드 폴더를 추가 (선택 사항)
    install(TARGETS retinify_py
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}/python
    )

endif()

# (참고: 상단에 남아있던 target_include_directories(retinify_py PRIVATE 는 삭제하거나 인자를 채워야 합니다.)
```

### Build

```
make build
cd build 
cmake .. -Dpybind11_DIR=/usr/local/lib/python3.10/dist-packages/pybind11/share/cmake/pybind11/
cmake --build . --config Release
```

## References
- [Retinify](https://retinify.ai/)
- [Retinify docs](https://docs.retinify.ai/index.html)
- [Retinify github](https://github.com/retinify/retinify)
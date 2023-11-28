---
layout: post
title: Install TensorRT in Docker Container
category: TensorRT
tag: tensorrt
---


#### 1. `https://developer.nvidia.com/nvidia-tensorrt-8x-download`에서 알맞는 버전의 tensorrt installer를 다운 받는다.

- `GA`가 안정된 버전이므로 추천
- tar 형식으로 다운로드

#### 2. 설치

1. 압축 풀기

```
tar xzvf <다운받은 tar 파일>
```

2. 설치하고자 하는 docker에 들어간다. 
```
docker exec -it -v <압축을 푼 path>:/TensorRT <container name> bash
```

3. package 설치
```
cd /TensorRT/python
pip install <tensorrt version>-<python version>-<linux>.whl
cd ../uff
pip install <uff>.whl
cd ../graphsurgeon
pip install <graphfsurgeon>.whl
```

4. `~/.bashrc`에 추가
```
export LD_LIBRARY_PATH=/TensorRT:/usr/local/cuda-<version>/lib64:
```

5. 설치 확인
```
dpkg -l | grep TensorRT
```

6. 파이썬에서의 라이브러리 다운로드
```
python -m pip install --upgrade setuptools pip
python -m pip install nvidia-pyindex
python -m pip install --upgrade nvidia-tensorrt
```

7. 파이썬에서 되는지 확인
```
python3
>>> import tensorrt
>>> print(tensorrt.__version__)
>>> assert tensorrt.Builder(tensorrt.Logger())
```
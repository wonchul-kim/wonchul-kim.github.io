---
layout: post
title: Install TensorRT in Docker 
category: TensorRT
tag: tensorrt
---

#### 1. 설치하고자 하는 docker에 들어간다. 
```
docker exec -it <container name> bash
```

#### 2. `https://developer.nvidia.com/nvidia-tensorrt-8x-download`에서 알맞는 버전의 tensorrt installer를 다운 받는다.

- `GA`가 안정된 버전이므로 추천
- `.deb`로 다운로드

#### 3. 설치

1. 

```
sudo dpkg -i <다운받은 파일 이름>
```

2. 이 때, 키를 등록하라는 메세지가 나온다면 등록한다.
```
apt-key add <메세지에서 나온 키값>
```

3. 
```
sudo apt-get udpate
sudo apt-get install tensorrt 
sudo apt-get install -y python3-libnvinfer-dev onnx-graphsurgeon
```

4. 설치 확인
```
dpkg -l | grep TensorRT
```

5. 파이썬에서의 라이브러리 다운로드
```
python -m pip install --upgrade setuptools pip
python -m pip install nvidia-pyindex
python -m pip install --upgrade nvidia-tensorrt
```

6. 파이썬에서 되는지 확인
```
python3
>>> import tensorrt
>>> print(tensorrt.__version__)
>>> assert tensorrt.Builder(tensorrt.Logger())
```
---
layout: post
title: Install PyCuda
category: TensorRT
tag: tensorrt
---

**Python**으로 Ubuntu에서 `onnx`를 `tensorrt`로 변환하기 위해서는 `pycuda`가 필요하다. 

설치는 다음과 같이 
```cmd
pip install pycuda
```

로 할 수 있지만, 가끔 다음과 같은 에러가 뜬다. 

```
............. : fatal error: pyconfig.h: no such file or directory
```

이 에러는 다음과 같이 
```
pip install python-dev
```

로 해결이 될 때도 있지만, 설치되어 있는 `python` 버전을 정확히 명시해주어야 `dev`가 설치가 되는 경우도 있어서
```
pip install python<version>-dev
```

로 설치해주는 것이 좋다.
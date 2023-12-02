---
layout: post
title: Install python
category: Ubuntu
tag: python
---

1. 저장소 추가
```
sudo apt install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
```

2. Install `python`
```
sudo apt install python<version> # python3.9
```

3. 설치되어 있는 `python` 확인
```
ls /usr/bin/python*
```

4. `python` config 설정
```
sudo update-alternatives --install /usr/bin/python python /usr/bin/python<version> 1
```

여기서, `/usr/bin/python`라고 설정하면, `python`으로 `python<version>`이 실행되는 link가 추가 된다고 보면 된다. 

5. `python` config 확인
```
sudo update-althernatives --config python
```






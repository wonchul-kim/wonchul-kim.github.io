---
layout: post
title: Install python env.
category: Ubuntu
tag: python
---

## Install `python`

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
sudo update-alternatives --install /usr/bin/python3 python /usr/bin/python<version> 1
```

여기서, `/usr/bin/python3`라고 설정하면, `python3`으로 `python<version>`이 실행되는 link가 추가 된다고 보면 된다. 

이 때, `python`으로 실행시키고 싶다면, symbolick link로 추가해준다.
```
ln -s /usr/bin/python3 /usr/bin/python
```

5. `python` config 확인
```
sudo update-alternatives --config python
```

6. install `python-dev`
```
sudo apt-get install python3-dev
```

Pytorch나 Tensorflow의 extension을 빌드할 때, `python3-dev`가 필요한 경우가 존재하기 때문에 설치해두는 것이 좋다. 

이 때, 설치되어 `python3-dev`로 설치하게 되면, 이미 설치되어 있는 `python`의 버전에 따라 자동으로 선택될 수 있으므로 아래와 같이 버전을 명시하는 것이 좋다.
```
sudo apt-get install python3.9-dev
```

## Install `pip`

```
sudo apt-get install python3-pip
```




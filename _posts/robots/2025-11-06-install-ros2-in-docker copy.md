---
layout: post
title: Install ROS2 in Docker
category: Robots
tag: [trt, tensorrt]
---


#### 1. 환경 설정 및 저장소 추가
* apt 업데이트 및 필수 도구 설치
```sh
apt update && apt install -y locales \
    && locale-gen en_US en_US.UTF-8 \
    && update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8 \
    && apt install -y curl gnupg lsb-release build-essential
```

* ROS 2 저장소 키 추가
```sh
curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```

* ROS 2 저장소 목록 추가
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null
```


#### 2. ROS 2 Humble 설치
```sh
apt update
apt install -y ros-humble-desktop
```

#### 3. 환경 활성화
```sh
# 현재 셸에 ROS 2 환경 설정
source /opt/ros/humble/setup.bash

# (선택 사항) 컨테이너에 다시 접속할 때마다 자동으로 소싱하도록 .bashrc에 추가
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
```
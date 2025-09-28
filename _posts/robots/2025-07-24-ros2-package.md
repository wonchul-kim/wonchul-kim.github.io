---
layout: post
title: How to make pkg
category: Robots
tag: [ros2, pkg]
---


#### Initialize ros2 pkg
```shell
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws
cd src
ros2 pkg create my_pkg --build-type ament_python --dependencies rclpy sensor_msgs std_msgs
```

#### Edit `setup.py`
```shell
# my_pkg/ros2_commander.py 로 파일 복사
# setup.py 내 entry_points에 다음 항목 추가
entry_points={
    'console_scripts': [
        'ros2_commander = my_pkg.ros2_commander:main',
    ],
},
```

#### Build pkg
```shell
cd ~/ros2_ws
colcon build
source install/setup.bash
```


#### Directories tree
```
ros2_ws/
└── src/
    └── my_pkg/
        ├── package.xml
        ├── setup.py
        └── my_pkg/                   ← 파이썬 모듈 디렉터리
            └── ros2_commander.py     ← 여기에 있어야 함
```
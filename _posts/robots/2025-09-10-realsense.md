---
layout: post
title: Realsense in ROS2
category: Robots
tag: [realsense]
---


# [Tutorial](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/realsense_setup.html)



## Issues

#### `realsense-viewer` 실행했는데, realsense 카메라가 인식되었음에도 뷰어에는 아무것도 잡히지 않는 경우

```sh
admin@wonchul:/workspaces/isaac_ros-dev$ realsense-viewer 

10/09 15:44:44,197 INFO [140625890492416] (context.cpp:116) ... 2-5-7 10/09 15:44:44,197 INFO [140625890492416] (context.cpp:128) Found 1 RealSense devices (0xff requested & 0xff from device-mask in settings)
```

위의 상태창을 보면, realsense device를 인식했음에도 뷰어에는 아무것도 잡히지 않음

* 해결법: udev 규칙을 호스트에 설치/갱신

```sh
sudo udevadm control --reload-rules && sudo udevadm trigger
```

## References:

* https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/realsense_setup.html

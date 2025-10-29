---
layout: post
title: ROS2 spacemouse
category: Robots
tag: [calibration]
---


# Spacemouse

### Install & Run
```sh
# install 
sudo apt install spacenavd
sudo apt install ros-indigo-spacenav-node
sudo apt install ros-humble-spacenav

# run
ros2 run spacenav spacenav_node

# check
ros2 topic list 

## 다음의 topic이 존재하는지 확인하고 echo로도 확인
# spacenav/offset (geometry_msgs/Vector3)
# spacenav/rot_offset (geometry_msgs/Vector3)
# spacenav/twist (geometry_msgs/Twist)
# spacenav/joy (sensor_msgs/Joy)
```


## References
- [spacenav_node - ROS Wiki](https://wiki.ros.org/spacenav_node)
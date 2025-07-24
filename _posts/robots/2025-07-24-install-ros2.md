---
layout: post
title: Install ROS2
category: Computer Vision
tag: [trt, tensorrt]
---



#### 1. Pull docker image
```shell
docker pull osrf/ros:humble-desktop
```

#### 2-1. Run docker image into container
```shell
docker run -it -d --privileged -e DISPLAY=$DISPLAY --env="QT_X11_NO_MITSHM=1" -v /tmp/.X11-unix:/tmp/.X11-unix:ro --hostname $(hostname) --network host --name <container name> osrf/ros:humble-desktop bash
```

#### 2-2. Set ros2

To use **ros2** command, NEED to do the below:

```shell
source /opt/ros/humble/setup.bash
```

Or, just edit `bashrc` file
```shell
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc && source ~/.bashrc
```

> Unless commit the modified content as docker image, whenever you access to the container, you need to type that command.


#### 2-3. To use gui from docker to local

Need to type the below in local env:
```shell
xhost +
```

> 로컬 환경에서 위의 명령어를 해주어야, 도커에서 실행되는 GUI를 로컬환경에서도 동시에 볼 수가 있다. 


#### 3. Test ros2 with `turtlesim`

* Run `turtlesim`
    ```shell
    docker exec -it <container name> bash
    source /opt/ros/humble/setup.bash
    ros2 run turtlesim turtlesim_node
    ```

* Run controller for `turtlesim`
    ```shell
    docker exec -it <container name> bash
    source /opt/ros/humble/setup.bash
    ros2 run turtlesim turtle_teleop_key
    ```
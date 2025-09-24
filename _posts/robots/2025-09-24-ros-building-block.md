---
layout: post
title: ROS2 Building Blocks
category: Robots
tag: [ros2, building block]
---


# ROS2 Building Blocks

* 스트리밍 데이터: **Topic**

* 즉시 응답형 요청: **Service**

* 오래 걸리고 취소/피드백 필요한 작업: **Action**

* 런타임 설정/튜닝: **Parameter** (+ /parameter_events)

* 좌표계/로봇 포즈 체계: **TF2**

* 안정적 bringup/정지: **Lifecycle**

* 데이터 수집/재현: **rosbag2**


#### Action

* 용도: 오래 걸리고 중간에 취소/진행상황 보고가 필요한 작업(예: 네비게이션, 매니퓰레이터 피킹)

* 구성: `goal(목표)`, `feedback(중간 진행)`, `result(최종 결과)` + `cancel`

* CLI: `ros2 action list`, `ros2 action info /<name>`, `ros2 action send_goal /<name> <pkg>/action/<Type> "{...}"`

#### Parameter

* 용도: 노드 런타임 설정값 관리(예: PID 게인, 토픽 이름, 파일 경로). 동적 변경 가능.

* 팁: 모든 파라미터 변경 이벤트는 `/parameter_events`로 방송됨.

* CLI: `ros2 param list|get|set|dump|load`

* YAML:
    ```yaml
    my_node:
    ros__parameters:
        kp: 1.2
        use_sim_time: true
    ```

#### TF2 (좌표 변환 프레임워크)

* 용도: 여러 좌표계 간 변환 트리 관리(카메라↔바디↔월드 등). 로봇/비전 필수.

* CLI/유틸:

    * 정적 변환 퍼블리시: `ros2 run tf2_ros static_transform_publisher x y z roll pitch yaw parent child`

    * 에코: `ros2 run tf2_ros tf2_echo base_link camera_link`

#### Lifecycle Node (관리형 노드)

* 용도: 노드의 상태를 단계적으로 관리(초기화→활성화→비활성화→정리). bringup/복구 시 안정적.

* 주요 상태/전이: `unconfigured → (configure) → inactive → (activate) → active → (deactivate/cleanup/shutdown)`

* CLI: `ros2 lifecycle set /<node> configure|activate|deactivate|cleanup|shutdown`

#### rosbag2 (레코드/재생)

* 용도: 디버깅·리그레이션·데이터셋 수집.

* CLI: `ros2 bag record -a, ros2 bag play <bag>, ros2 bag info <bag>`

* 팁: 저장 백엔드(SQLite2/MCAP 등), 압축 옵션, 재생 시 QoS 재정의 가능.
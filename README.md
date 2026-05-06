# TwinFlight Mission v2

ROS2·PX4 기반 자율비행 드론 발표용 미니 프로젝트입니다.

v1이 단순히 ROS2에서 PX4로 Offboard 명령을 보내는 데모였다면, v2는 **시설 점검용 자율비행 임무 시뮬레이터** 형태로 구체화했습니다.

## 프로젝트 목적

컴퓨팅사고 팀 발표에서 다음 메시지를 코드 구조로 보여주는 것이 목표입니다.

> ROS2는 임무 경로 생성, 상태 관리, 상위 자율 기능을 담당하고, PX4는 자세 제어, 위치 제어, 고도 유지, 모터 제어 등 실제 비행 안정화를 담당한다.

## 구현 시나리오

**시설 점검용 자율비행 시나리오**

드론이 시설물을 점검한다고 가정하고, `mission.yaml`에 정의된 waypoint를 순서대로 이동합니다.

```text
1. Arm
2. Offboard 모드 전환
3. 이륙 고도 5m 유지
4. 점검 지점 1 이동
5. 점검 지점 2 이동
6. 복귀 지점 이동
7. Hover 또는 종료
```

## 구조

```text
mission.yaml
    ↓
Mission Planner
    ↓
PX4 Offboard Mission Node
    ↓
/fmu/in/offboard_control_mode
/fmu/in/trajectory_setpoint
/fmu/in/vehicle_command
    ↓
uXRCE-DDS
    ↓
PX4 SITL
    ↓
Gazebo Drone
```

## 권장 환경

- Ubuntu 22.04
- ROS2 Humble
- PX4 Autopilot v1.14 이상 또는 main
- PX4 SITL + Gazebo
- Micro XRCE-DDS Agent
- QGroundControl

## 실행 순서

### 1. PX4 SITL 실행

```bash
cd ~/PX4-Autopilot
make px4_sitl gz_x500
```

### 2. Micro XRCE-DDS Agent 실행

```bash
MicroXRCEAgent udp4 -p 8888
```

### 3. ROS2 워크스페이스 빌드

```bash
cd ~/twinflight-mission-v2/ros2_ws
colcon build
source install/setup.bash
```

### 4. Mission v2 실행

```bash
ros2 launch twinflight_mission mission.launch.py
```

또는 직접 실행:

```bash
ros2 run twinflight_mission mission_node --ros-args -p mission_file:=src/twinflight_mission/config/mission.yaml
```

## 발표용 캡처 추천

1. `mission.yaml` 경로 설정 화면
2. `mission_node.py` 코드 화면
3. Gazebo/PX4 SITL 드론 화면
4. QGroundControl 상태 화면
5. `rqt_graph` 노드 구조 화면
6. 터미널 mission log 화면

## 주의

이 프로젝트는 실제 기체 비행용이 아니라 발표용 SITL/Gazebo 시뮬레이션 구조 예시입니다.
실제 드론에 연결하지 마세요.

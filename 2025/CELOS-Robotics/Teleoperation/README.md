# 🤖 VR Teleoperation Setup | CELOS Lab

> **ROBOTIS ai_worker 로봇 VR 원격 조작 인프라 구축**
> **기간:** 2026.06
> **역할:** VR-ROS2 원격 조작 셋업 매뉴얼 제작, WebXR-ROS2 통신 파이프라인 구축·검증, Isaac Sim 연동 트러블슈팅 (팀원과 공동 진행)

---

## 📌 Executive Summary

* **VR-ROS2 원격 조작 셋업 매뉴얼 제작**
  * Docker(ROS2 Jazzy) + ngrok 기반 VR 헤드셋-로봇 통신 환경 구축 과정을 재현 가능한 매뉴얼로 정리
  * Vuer(WebXR) 핸드 트래킹 데이터를 ROS2 토픽(`/left_hand/joint_trajectory` 등)으로 발행하는 구조 구현
  * 손을 움직이면 실시간으로 좌표값이 토픽에 발행되는 것을 터미널에서 직접 검증

* **Isaac Sim ROS2 Bridge 연동**
  * FastDDS 프로필 설정으로 별도 도커 컨테이너 간 DDS 통신 정합
  * OmniGraph Action Graph 구성 (ROS2 Publish/Subscribe Joint State, Articulation Controller)
  * Joint Angular Drive 파라미터 일괄 적용 스크립트 작성 (Python, USD API)

* **팀원과 공동으로 최종 연결 성공**
  * Articulation Root 위치 재배치, 좌표값→관절값 매핑 구조 문제 규명
  * VR 핸드 트래킹 동작이 Isaac Sim 로봇에 실시간 반영되는 것까지 팀원과 함께 성공

---

## 🛠 시스템 구성

| 구성 요소 | 역할 |
|-----------|------|
| VR 헤드셋 (Web browser) | 손 동작 캡처, Vuer 웹페이지 접속 |
| ngrok | 로컬 ROS2 서버를 외부(VR 헤드셋)에서 접근 가능하게 터널링 |
| Docker (ROS2 Jazzy) | ai_worker 패키지 실행 환경, VR-ROS2 브릿지 노드 구동 |
| Isaac Sim (ROS2 Bridge) | 로봇 모델 시뮬레이션, Joint 제어 |
| FastDDS | 서로 다른 컨테이너(ai_worker ↔ Isaac Sim) 간 DDS 통신 정합 |

**데이터 흐름:** VR 손동작 → Vuer(WebXR) → ROS2 토픽 발행 → (FastDDS 통신) → Isaac Sim OmniGraph → Articulation Controller → 로봇 Joint 제어

---

## 🔧 트러블슈팅 및 원인 분석

| 문제 | 원인 분석 | 해결 |
|------|-----------|------|
| VR 웹 페이지에서 손 데이터 송출 실패 | 인증서 없는 데이터로 브라우저가 차단하는 것으로 추정 | Cloudflare Tunnel 대안 검토, 내부망 우선 테스트로 방향 전환 |
| 로봇 하체는 반응하나 팔이 경직 | Articulation Root가 `world`에 걸려있어 하위 관절 제어가 안 되는 구조적 문제 | 최상위 로봇 prim(`/ffw_sg2_follower`)으로 Articulation Root 재배치, Action Graph Target Prim 재지정 |
| 재배치 후 로봇이 부자연스럽게 흔들림 | Collision 설정과 충돌 | Collision 해제 시 자연스러운 움직임 확인 |
| VR 좌표는 갱신되나 Isaac Sim 로봇 반응 없음 | 좌표값이 아닌 관절값 기반으로 통신해야 하는 근본 구조 문제 | 팀원과 함께 관절 매핑 방식으로 전환하여 최종 연결 성공 |

---

## 🎥 Evidence

### 실제 동작 확인

**VR 헤드셋 패스스루 화면 — 손 스켈레톤 실시간 트래킹**

![vr passthrough](<관련자료/01_vr_passthrough_hand_skeleton.png>)

*▲ Meta Quest 패스스루로 실제 작업 데스크가 보이는 동시에, 손 관절 트래킹(빨강/초록/파랑 스켈레톤)이 실시간으로 겹쳐 표시되는 것을 확인.*

**VR 그리드 환경 — 핸드 트래킹 동작 GIF**

![hand tracking demo](<관련자료/02_hand_tracking_grid_demo.gif>)

*▲ 손을 움직이면 VR 공간 내 스켈레톤이 실시간으로 따라 움직이는 것을 확인.*

---

### 통신 파이프라인 구성

| 화면 | 설명 |
|------|------|
| ![vuer interface](<관련자료/03_vuer_webxr_interface.png>) | Vuer(WebXR) 인터페이스 — Hands 컨트롤 패널, FPS/Stream 설정 |
| ![topic echo](<관련자료/04_joint_trajectory_topic_echo.png>) | `/left_hand/joint_trajectory` 토픽 echo — 손 움직임에 따라 실시간으로 좌표값이 발행되는 것을 직접 검증 |
| ![topic list](<관련자료/05_ros2_topic_list.png>) | `ros2 topic list` — VR 관련 토픽들(`/vr_hand/*`, `/vr_control/toggle` 등)이 정상 발행되는 것 확인 |

---

### Isaac Sim 연동 구성

| 화면 | 설명 |
|------|------|
| ![omnigraph](<관련자료/06_omnigraph_action_graph.png>) | OmniGraph Action Graph — ROS2 Publish/Subscribe Joint State, Articulation Controller 노드 연결 |
| ![joint params](<관련자료/07_joint_angular_drive_params.png>) | Joint Angular Drive 파라미터 — Python/USD API로 일괄 적용한 Damping·Stiffness 값 |
| ![robot mesh](<관련자료/08_isaac_sim_robot_collision_mesh.png>) | Isaac Sim 내 ai_worker 로봇, collision mesh(초록 와이어프레임) 표시 |
| ![physics scene](<관련자료/09_physics_scene_settings.png>) | Physics Scene 설정 — Gravity, Solver Type(TGS), CCD 활성화 등 |
---

## ✅ 결론

VR 헤드셋의 손동작이 ROS2 토픽으로 안정적으로 발행되는 통신 계층을 구축·검증했고, Isaac Sim 연동 과정에서 발생한 구조적 문제(Articulation Root 위치, 좌표-관절 매핑 불일치)를 직접 규명해 팀의 최종 연결 성공에 기여했습니다. 이 경험을 통해 이기종 시스템(VR·ROS2·시뮬레이터) 간 데이터 파이프라인을 설계하고, 실패 지점을 좌표계·통신 프로토콜 단위로 추적해 원인을 좁혀가는 디버깅 역량을 쌓았습니다.

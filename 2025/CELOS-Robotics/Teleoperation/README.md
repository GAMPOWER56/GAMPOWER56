# 🤖 VR Teleoperation Setup | CELOS Lab (2026.06)

> **ROBOTIS ai_worker 로봇 VR 원격 조작 인프라 구축**
> **역할:** 초기 셋업 인수인계 정리, WebXR-ROS2 통신 파이프라인 구축·검증, Isaac Sim 연동 트러블슈팅 (팀원과 공동 진행)

---

## 📌 Executive Summary

* **VR-ROS2 통신 파이프라인 구축 및 검증**
  * Docker(ROS2 Jazzy) + ngrok 기반 VR 헤드셋-로봇 통신 인프라 셋업
  * Vuer(WebXR) 핸드 트래킹 데이터를 ROS2 토픽(`/left_hand/joint_trajectory` 등)으로 발행하는 구조 구현
  * 손을 움직이면 실시간으로 좌표값이 토픽에 발행되는 것을 터미널에서 직접 검증

* **Isaac Sim ROS2 Bridge 연동**
  * FastDDS 프로필 설정으로 별도 도커 컨테이너 간 DDS 통신 정합
  * OmniGraph Action Graph 구성, Joint Angular Drive 파라미터 일괄 적용 스크립트 작성
  * Articulation Root 위치 재배치, 좌표값→관절값 매핑 구조 문제 규명 등 팀원과 공동 디버깅
  * **VR 핸드 트래킹 동작이 Isaac Sim 로봇에 실시간 반영되는 것까지 팀원과 함께 성공적으로 연결**

* **트러블슈팅 및 원인 분석**

| 문제 | 분석 내용 |
|------|-----------|
| VR 웹 페이지에서 손 데이터 송출 실패 | 인증서 없는 데이터로 브라우저가 차단하는 것으로 추정, 대안 경로 검토 |
| 로봇 하체는 반응하나 팔이 경직 | Articulation Root가 world에 걸려있어 하위 관절 제어가 안 되는 구조적 문제 확인 |
| VR 좌표는 갱신되나 Isaac Sim 로봇 반응 없음 | 좌표값이 아닌 관절값 기반으로 통신해야 하는 근본 원인 규명 → 팀원과 함께 관절 매핑 방식 전환하여 해결 |

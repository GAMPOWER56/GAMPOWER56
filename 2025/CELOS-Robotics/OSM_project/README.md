# 🗺️ OSMnx & GA-Based Multi-Destination Path Optimization | CELOS Lab

> **ICROS 2026 학술대회 투고 논문 연구**  
> **주제:** OSMnx 및 유전 알고리즘(GA) 기반 다중 목적지 실시간 경로 최적화 시스템 구현 및 성능 분석  
> **저자:** 이다겸(제1저자), 한승호(교신저자 / 한양대학교 ERICA 전자공학부)

---

## 📑 Paper & Poster Downloads

본 연구의 공식 투고 논문 원본 및 학술대회 발표 포스터 자료입니다.

* 📄 **[ICROS 2026 학술대회 투고 논문.PDF](https://github.com/user-attachments/files/30921181/ICROS_2026_.-._.pdf)**

* 📊 **[ICROS 2026 발표 포스터.pdf](https://github.com/user-attachments/files/30921185/ICROS_poster_._.pdf)**

---

## 📌 Executive Summary

* **OpenStreetMap(OSM) 기반 도로망 그래프 모델링**
  * 한양대학교 ERICA 캠퍼스 중심 ROI(반경 700m) 도로망 데이터를 OSMnx로 추출하여 교차로(Node)와 도로(Edge) 그래프 구축
  * 도로 법정 제한 속도 기반 통행 소요 시간($T_{total}$)을 간선 가중치(Weight)로 할당
* **유전 알고리즘(Genetic Algorithm) 기반 최적 방문 순서 산출**
  * 다중 목적지 방문 순서를 정수 염색체로 인코딩하고, 총 이동 시간의 역수를 적합도 함수로 정의
  * 엘리트 보존(Elitism), 순서 교차(Ordered Crossover), 스왑 변이(Swap Mutation) 적용 (Population Size: 50, Generations: 100)
* **실시간 동적 우회(Dynamic Rerouting) 및 확장성 평가**
  * 특정 노드 가중치를 100배 증폭한 교통 체증 시뮬레이션을 통해 체증 구간 실시간 우회 경로 생성 검증
  * 최대 100개 목적지 환경에서 **평균 0.33초 이내**의 실시간 연산 응답성 확보 및 300개 노드 스트레스 테스트(2.53초)를 통한 단일 GA 모델 연산 임계점 도출

---

## 📊 Performance Benchmarks

| 목적지 수 (Nodes) | 평균 연산 시간 (s) | 최솟값 (s) | 최댓값 (s) |
|:---:|:---:|:---:|:---:|
| **3개** | 0.0235 | 0.0216 | 0.0287 |
| **5개** | 0.0386 | 0.0263 | 0.0559 |
| **7개** | 0.0348 | 0.0317 | 0.0382 |
| **10개** | 0.0335 | 0.0247 | 0.0508 |
| **50개** | 0.1380 | 0.1302 | 0.1448 |
| **100개** | **0.3307** | 0.3285 | 0.3339 |
| **300개** | 2.5267 | 2.0469 | 3.0417 |

* **실시간성 임계점:** 100개 노드 이하에서는 0.34초 이내의 즉각적인 경로 재계산 가능
* **한계점 및 확장성:** 300개 대규모 노드 환경에서는 단일 에이전트 연산 지연(2.53s)이 관찰되어, 향후 군집 로봇(Swarm Robotics) 및 계층적 최적화(Hierarchical Planning) 연구 필요성 도출

---

## 📸 Research Artifacts

<table align="center">
  <tr>
    <td align="center" width="50%">
      <img width="1235" height="1230" alt="image" src="https://github.com/user-attachments/assets/28c9b13f-10c7-47c0-81d0-de7d09f1364a" /><br>
      <sub><b>Dynamic Rerouting Comparison</b><br>교통 체증 유도 시 동적 우회 경로 생성 (Red: 기존 / Blue: 우회)</sub>
    </td>
    <td align="center" width="50%">
      <img width="1224" height="1218" alt="스크린샷 2026-04-29 230545" src="https://github.com/user-attachments/assets/8a003a4e-2ed6-494c-9b1d-f69b5b74e2c0" /><br>
      <sub><b>Multi-Destination Path Optimization</b><br>10개 목적지 환경에서의 GA 기반 최적 방문 순서 산출</sub>
    </td>
  </tr>
</table>

---

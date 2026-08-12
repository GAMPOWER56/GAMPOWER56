# 🔬 Simulation Notes | IIT Guwahati (2026.01 ~ 02)

Lumerical FDTD · MODE를 활용한 실리콘 포토닉스 광소자 시뮬레이션 실습 기록입니다.
1차(직선 도파관)부터 시작해 소자 복잡도를 높여가며 진행했고, Fab 탐방에서 시뮬레이션과 실제 공정의 간극을 직접 확인했습니다.

---

## 시뮬레이션 목록

| 차수 | 소자 | 핵심 내용 | 결과 | 노트 |
|------|------|-----------|------|------|
| 1차 | 직선 웨이브가이드 | varFDTD 기본 세팅, Monitor 3종 설정, Convergence Test | 투과율 90% → **92%** | [📄](<../관련자료/1차 직선 웨이브가이드_260811_095048.pdf>) |
| 2차 | 굴은 웨이브가이드 | Euler Bend 구조, 오일러 밴드 스크립트 작성 | Bend Loss 측정, 투과율 70~80% | [📄](https://github.com/user-attachments/files/30918976/2._260811_095043.pdf) |
| 3차 | 방향성 결합기 (DC) | Evanescent wave 결합 원리, Gap 200nm 최적화 | 결합 확인 | [📄](https://github.com/user-attachments/files/30918983/3.DC_260811_095037.pdf) |
| 4차 | Y-Branch | S-parameter 추출, INTERCONNECT 연계, MZI 구성 요소 | T = 0.99% (50:50 분배) | [📄](https://github.com/user-attachments/files/30919002/4.YBranch_260811_095034.pdf) |
| 5차 | 링 공진기 (RR) | Over/Critical/Under Coupling 비교, Gap 최적화 | Gap 0.1→0.2→0.3nm 에 따른 coupling 상태 변화 | [📄](https://github.com/user-attachments/files/30919004/5.Ring.resonator_260811_095030.pdf) |
| 6차 | 단열 분할기 (Adiabatic Splitter) | DC보다 안정적인 광 이동, 광 간섭 없이 50:50 분배 | — | [📄](https://github.com/user-attachments/files/30919012/6.Adiabatic.Splitter_260811_095029.pdf) |
| 7차 | MZI + RR | MZI + RR 결합, INTERCONNECT 광 변조기 설계 | -5.5dB까지 내려가는 Drop 확인 | [📄](https://github.com/user-attachments/files/30919014/7.MZIRR_260811_095027.pdf) |
| 8차 | 링 공진기 센서 (RR sensor) | SiO₂ → H₂O(n=1.33) cladding 변경, 굴절률 변화 감지 | KLayout 선지 및 INTERCONNECT 연동 | [📄](https://github.com/user-attachments/files/30919024/8.RR.sensor_260811_095026.pdf) |
| 9차 | Grating Coupler | 격자 주기 0.63μm, Etch Depth 70nm, 광원 각도 최적화 | 결합 효율 1.62% → **80%** | [📄](https://github.com/user-attachments/files/30919027/9.Grating.coupler_260811_095014.pdf) |
| 10차 | 링 공진기 레이스트랙 | Radius 8~12μm 파라미터 스윕, FSR·Q-factor·ER 분석 | Q-factor 최대 **190** (R=12μm) | [📄](https://github.com/user-attachments/files/30919030/10.RR.racetrack_260811_095024.pdf) |
| 11차 | 비대칭 MZI | ΔL 50~110μm FSR 분석, FSR ∝ 1/ΔL 검증 | Q-factor 최대 **195** (ΔL=110μm) | [📄](https://github.com/user-attachments/files/30919031/11.MZI_260811_095020.pdf) |
| Fab 탐방 | 팹실 방문 | 공정 장비 실습, Probe Station 계측, 트러블슈팅 3건 | 시뮬레이션(95%) vs 실제 소자(60%) 투과율 오차 직접 확인 | [📄](https://github.com/user-attachments/files/30919055/Fab._260811_095015.pdf) |

---

## 핵심 정량 데이터

**Grating Coupler (9차)**
- 초기 투과율: 1.62% → 최적화 후 **80%** 달성
- 구조: Period 0.63μm, Etch Depth 70nm, 광원 각도 8~12°

**Ring Resonator Radius Sweep (10차)**

| Radius | FSR (nm) | FWHM (nm) | Q-factor | ER (dB) |
|--------|----------|-----------|----------|---------|
| 8μm | 16.96 | 12.29 | 106 | 35.77 |
| 9μm | 15.11 | 11.00 | 140 | 35.41 |
| 10μm | 13.87 | 10.05 | 158 | 7.70 |
| 11μm | 12.83 | 9.30 | 171 | 7.24 |
| 12μm | 11.58 | 8.39 | **190** | 7.60 |

**비대칭 MZI ΔL Sweep (11차)**

| ΔL (μm) | FSR (nm) | Q-factor |
|---------|----------|----------|
| 50 | 21.66 | 96.2 |
| 70 | 16.34 | 126.6 |
| 90 | 12.62 | 164.3 |
| 110 | 10.63 | **195.3** |

→ FSR ∝ 1/ΔL 이론값과 일치 확인

---

## Fab 탐방에서 배운 것

공정 흐름: Cleaning → Spin Coater → Soft Baking → Metalization (PVD) → 리소그래피 → Characterization

**트러블슈팅 경험**

| 문제 | 원인 | 해결 |
|------|------|------|
| 빛/전기 신호 안 잡힘 (얼라인먼트 실패) | 실리콘 포토닉스 소자는 1~2μm만 벗어나도 빛이 새어나감 | X·Y·Z축 미세 조작으로 Sweet Spot 탐색 |
| I-V 그래프 노이즈 심함 (접촉 불량) | 전극 표면 자연 산화막 형성 | Skating/Scrubbing 기법으로 산화막 제거 |
| 시뮬레이션 vs 실제 불일치 | 공정 중 선폭·두께의 물리적 오차 | 측정 데이터로 Lumerical 파라미터 피드백 |

---

## 사용 툴

| 툴 | 용도 |
|----|------|
| Lumerical FDTD | 3D 전자기장 시뮬레이션 (투과율, 반사율, 필드 분포) |
| Lumerical MODE | 도파관 단면 분석 (n_eff, Group Index, Bend Loss) |
| Lumerical INTERCONNECT | S-parameter 기반 광회로 시뮬레이션 |
| Lumerical Script (.lsf) | FSR·Q-factor·ER 자동 추출 자동화 |

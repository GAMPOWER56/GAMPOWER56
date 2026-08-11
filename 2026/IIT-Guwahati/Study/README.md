# 📚 Study Notes | IIT Guwahati 인턴십 (2026.01 ~ 02)

인턴십 기간 동안 실리콘 포토닉스 소자 설계를 위해 학습한 내용을 정리한 노트입니다.
시뮬레이션 실습과 병행하며 작성했습니다.

---

## 파일 목록

| 파일 | 내용 | 비고 |
|------|------|------|
| [01_semiconductor_physics.pdf](https://github.com/user-attachments/files/30918714/_260811_095059.pdf) | 반도체 물리 기초 — Energy Band, p-n Junction, LED | 소자 설계 전 필수 이론 |
| [02_lumerical_MODE.pdf](https://github.com/user-attachments/files/30918738/Lumerical.MODE_260811_095038.pdf) | Lumerical MODE 사용법 — 도파관 유효굴절률, Group Index 계산 | 도파관 설계 실습 병행 |
| [03_lumerical_FDTD.pdf](https://github.com/user-attachments/files/30918741/Lumerical.simulator_260811_095053.pdf) | Lumerical FDTD 사용법 — 시뮬레이션 세팅, Convergence Test, S-parameter 추출 | Grating Coupler 최적화에 활용 |
| [참고논문.pdf](https://github.com/user-attachments/files/30918753/Safa.O.Kasap.Optoelectronics.Photonics_.Princ_260811_095107.pdf) |

---

## 학습 흐름

```
반도체 물리 기초 (소자 동작 원리 이해)
    ↓
Lumerical MODE (도파관 단면 분석 → 유효굴절률, Bend Loss 계산)
    ↓
Lumerical FDTD (3D 전자기장 시뮬레이션 → 투과율, 반사율, 필드 분포 확인)
```

---

## 핵심 내용 요약

**반도체 물리**
- Energy Band, Fermi-Dirac 분포, p-n Junction 동작 원리
- Direct/Indirect Bandgap, LED 발광 메커니즘
- 실리콘 포토닉스 소자 설계의 물리적 기반

**Lumerical MODE**
- FDE Solver로 도파관 유효굴절률(n_eff), Group Index(n_g), Dispersion 계산
- Convergence Test — Mesh 크기, Simulation Span 최적화
- Si/SiO₂ 도파관 TE/TM 모드 분석

**Lumerical FDTD**
- Standard Workflow — Setup → Run → Post-processing → Iteration
- Boundary Conditions (PML), Mesh Override 설정
- S-parameter 추출 → Lumerical INTERCONNECT 회로 시뮬레이션 연계
- Q-factor, FSR, Extinction Ratio 분석.

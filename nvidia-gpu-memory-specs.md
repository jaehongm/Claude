# NVIDIA GPU 세대별 메모리 Capacity & Bandwidth 정리

> 조사 기준일: 2026년 3월 30일
> H100 세대부터 Feynman까지 전 세대 정리

---

## 중요 사전 참고: G1–G4 용어 관련 명확화

NVIDIA 공식 문서 및 WEKA/Vast Data 등 파트너 자료에서 **G1–G4는 GPU 칩 세대가 아니라 추론 시스템 내 메모리/스토리지 계층(Tier)을 의미**합니다.

| Tier | 미디어 | 역할 |
|------|--------|------|
| G1 | GPU HBM | 활성 추론 중 Hot KV Cache (레이턴시 최소) |
| G2 | 시스템 DRAM | HBM 오프로드용 스테이징/버퍼 |
| G3 | 로컬 NVMe SSD | 단기 재사용 Warm KV Cache |
| G3.5 | 이더넷 연결 NVMe Flash (ICMS/CMX) | Pod 규모 KV Cache 오프로드 *(2026년 신규 추가)* |
| G4 | 공유 네트워크 스토리지 (VAST, Weka) | Cold 아티팩트, 영구 KV Cache 보관 |

> G3.5 티어는 NVIDIA의 BlueField-4 기반 **ICMS(Inference Context Memory Storage)** 플랫폼으로 CES 2026에서 신규 도입.
> Vast Data, Weka는 G4(및 G3.5) 스토리지 파트너로서 GPU 칩 세대 스펙과는 무관.

아래 본 문서에서는 GPU 칩 **세대를 G1(H100)→G2(Blackwell)→G3(Rubin)→G3.5(Rubin Ultra)→G4(Feynman)**로 편의상 구분하여 정리합니다.

---

## G1: H100 세대 (Hopper 아키텍처, 2022–2024)

### 개별 GPU 스펙

| 모델 | 메모리 타입 | 용량/GPU | 대역폭/GPU | TDP | 비고 |
|------|------------|---------|-----------|-----|------|
| H100 SXM5 | HBM3 | 80 GB | 3.35 TB/s | 700 W | 5,120-bit 버스; NVLink 4.0 |
| H100 PCIe | HBM2e | 80 GB | ~2.0 TB/s | 350 W | NVLink 없음 |
| H100 NVL (듀얼카드) | HBM3 | 94 GB/GPU (카드 총 188 GB) | 3.94 TB/s/GPU | 400 W (카드) | 6,016-bit 버스; 2-GPU 브리지 카드 |
| H200 SXM | HBM3e | 141 GB | 4.8 TB/s | 700 W | HBM3e 24GB 스택 6개; NVLink 4.0 |
| H200 NVL (듀얼카드) | HBM3e | 141 GB/GPU (카드 총 282 GB) | 4.8 TB/s/GPU | — | H100 NVL 폼팩터 동일; 메모리만 교체 |

> **H100→H200 업그레이드:** 다이, CUDA 코어, 텐서 코어 동일. HBM3(80GB, 3.35TB/s) → HBM3e(141GB, 4.8TB/s) 메모리만 교체.

### NVL 시스템 (H100/H200 DGX)

| 시스템 | GPU 수 | 총 GPU 메모리 | 총 HBM BW | NVLink 버전 | 비고 |
|--------|--------|-------------|-----------|------------|------|
| DGX H100 | 8× H100 SXM | 640 GB | 26.8 TB/s | NVLink 4.0 | 노드 내 전 GPU 연결; 노드간 NVLink Switch로 최대 256 GPU |
| DGX H200 | 8× H200 SXM | 1,128 GB (1.1 TB) | 38.4 TB/s | NVLink 4.0 | — |

> H100/H200 세대는 GB200 NVL72 같은 전용 랙 스케일 NVL 시스템 없음.

---

## G2: Blackwell 세대 (GB200 / B200, 2025)

### 개별 GPU 스펙

| 모델 | 메모리 타입 | 용량/GPU | 대역폭/GPU | TDP | 비고 |
|------|------------|---------|-----------|-----|------|
| B200 SXM | HBM3e | 192 GB | ~8 TB/s | ~1,000 W | HBM3e 스택 8개; 8,192-bit 인터페이스 |
| GB200 Superchip | HBM3e | 192 GB (B200 2개/수퍼칩) | ~8 TB/s/B200 | — | B200 2개 + Grace CPU 1개; NVLink-C2C 900 GB/s |
| B300 (Blackwell Ultra) | HBM3e | 288 GB | 8 TB/s | 1,400 W | HBM3e 12-Hi 스택 8개; PCIe Gen6 |
| GB300 Superchip | HBM3e | 288 GB/B300 | 8 TB/s/B300 | — | B300 + Grace CPU |

### NVL 랙 시스템

| 시스템 | GPU 패키지 수 | CPU | 총 GPU 메모리 | 총 HBM BW | NVLink Fabric BW | FP4 성능 | 전력 |
|--------|-------------|-----|-------------|-----------|-----------------|---------|------|
| GB200 NVL36 | 36× B200 | 18× Grace | ~6.9 TB | ~288 TB/s | ~65 TB/s | ~0.72 EFLOPS | ~60 kW |
| GB200 NVL72 | 72× B200 | 36× Grace | 13.8 TB | ~576 TB/s | 130 TB/s | 1.44 EFLOPS | ~120 kW |
| GB300 NVL72 | 72× B300 | 36× Grace | 20.7 TB | 576 TB/s | 130 TB/s | 1.1 EFLOPS (dense) | ~132–140 kW |

> **"240 TB 신속 접근 메모리"** 수치: 576-GPU NVLink 도메인에서 Grace CPU LPDDR5X 메모리까지 통합한 수치.
> GB200 NVL72에서 각 GPU는 NVLink 5세대로 타 GPU 메모리를 1.8 TB/s 속도로 접근 가능.

---

## G3: Vera Rubin 세대 (Rubin 아키텍처, 2026 H2)

### 개별 GPU 스펙

| 모델 | 메모리 타입 | 용량/패키지 | 대역폭/패키지 | TDP | 비고 |
|------|------------|-----------|------------|-----|------|
| R200 (Vera Rubin, 듀얼-다이) | HBM4 | 288 GB | 20.5–22 TB/s | ~600 W+ | HBM4 스택 8개 @ 10.8 GT/s; 2-다이 패키지; TSMC 3nm |
| Rubin CPX (단일-다이, Prefill 전용) | GDDR7 | 128 GB | ~2 TB/s | 낮음 | 512-bit 버스 @ 32 Gbps; FP4 30 PFLOPS; PCIe Gen6, NVLink 없음 |

> **HBM4 대역폭 주의:** NVIDIA가 JEDEC 표준 이상의 핀 속도 요청. 초도 양산(2026)은 ~20–20.5 TB/s, 목표 스펙 22 TB/s(10.8 GT/s). SemiAnalysis에 따르면 공급사 달성에 어려움 있음.

### NVL 랙 시스템

| 시스템 | 구성 | CPU | 총 GPU 메모리 | 총 HBM BW | NVLink 6 BW | FP4 성능 | 전력 |
|--------|------|-----|-------------|-----------|------------|---------|------|
| Vera Rubin NVL72 | 72× R200 패키지 (144 다이) | 36× Vera | ~20.7 TB | ~1.4 PB/s | 260 TB/s | 3.6 EFLOPS | ~300 kW |
| Vera Rubin NVL144 CPX (콤보) | 144× R200 + 144× CPX | 36× Vera | 100 TB (fast) | 1.7 PB/s | — | 8 EFLOPS (콤보) | — |

### NVL 명칭 변경 이력

| 시기 | 명칭 | 설명 |
|------|------|------|
| 초기 발표 (GTC 2025) | NVL144 | 72 패키지 × 2 다이 = 144 GPU 다이 기준 명명 |
| 최종 확정 (2025 12월/CES 2026) | **NVL72** | GPU **패키지** 72개 기준으로 변경 |
| 콤보 랙 | **NVL144** | R200 + CPX 혼합 144 패키지 랙에 NVL144 명칭 재사용 |

> **Vera CPU:** Olympus 코어 88개 (176 스레드); Grace 대비 2× 성능; NVLink-C2C로 R200 2개와 연결.

---

## G3.5: Vera Rubin Ultra 세대 (Rubin Ultra 아키텍처, 2027 H2)

### 개별 GPU 스펙

| 모델 | 메모리 타입 | 용량/패키지 | 대역폭/패키지 | TDP | 비고 |
|------|------------|-----------|------------|-----|------|
| R300 (Rubin Ultra) | HBM4e | 1,024 GB (1 TB) | 32 TB/s | 3,600 W/패키지 | HBM4e 스택 16개; 4-다이 패키지; TSMC CoWoS-L |

### NVL 랙 시스템

| 시스템 | GPU 패키지 수 | 총 GPU 메모리 | 총 HBM BW | NVLink 7 BW | FP4 성능 | 전력 |
|--------|-------------|-------------|-----------|------------|---------|------|
| Rubin Ultra NVL576 (Kyber 랙) | 576× R300 | ~576 TB | ~4.6 PB/s | 1.5 PB/s | 15 EFLOPS | ~600 kW |

> **G3 vs G3.5 비교:**
> - 패키지당 메모리: 288 GB → 1,024 GB (3.56×)
> - 패키지당 대역폭: 20.5 TB/s → 32 TB/s (~1.6×)
> - NVL 규모: NVL72 → NVL576 (8×)
> - FP4 성능: 3.6 EFLOPS → 15 EFLOPS (~4.2×)
> G3.5로 분류하는 이유: Rubin 아키텍처 동일하나 4-다이 패키지 + HBM4e 업그레이드로 브리지 세대 성격.

---

## G4: Feynman 세대 (2028)

> **주의:** Feynman은 GTC 2025/2026 로드맵에서 확인된 아키텍처이나, 정확한 메모리 용량/대역폭 수치는 아직 공식 발표되지 않음. 아래는 NVIDIA 공개 정보 + 업계 분석가 추정치 혼합.

### 개별 GPU 스펙 (추정 포함)

| 모델 | 메모리 타입 | 용량/패키지 | 대역폭/패키지 | TDP | 상태 |
|------|------------|-----------|------------|-----|------|
| F400 (Feynman) | Custom HBM (HBM5 유력) | ~400–640 GB | ~32 TB/s | ~4,400 W (추정) | 2028 확정; TSMC A16(1.6nm) |

**HBM5 예상 스펙 (JEDEC 로드맵 기준):**
- 스택당 I/O: 4,096-bit
- 스택당 대역폭: ~4 TB/s
- 스택 높이: 16-Hi; 스택당 용량: ~80 GB
- 패키지당 스택 8개 → ~640 GB (일부 분석은 400–500 GB 추정; 사용 스택 변형에 따라 달라짐)

### NVL 랙 시스템 (추정)

| 시스템 | GPU 패키지 수 | NVL 인터커넥트 | FP4 성능 (추정) | 전력 (추정) |
|--------|-------------|--------------|--------------|-----------|
| Feynman NVL576 | 576× F400 | NVLink 8 + Co-packaged Optical | TBD (>>15 EFLOPS) | >1.2 MW |
| Feynman NVL1152 | 1,152× F400 | NVLink 8 광학 스케일아웃 | TBD | TBD |

### Feynman 핵심 혁신 사항

1. **TSMC A16 (1.6nm)** — 2nm 스킵; GAA 트랜지스터 + High-NA EUV
2. **Silicon Photonics (CPO)** — NVLink 스위치에 공동 패키징 광학; NVIDIA 최초 CPO NVLink 플랫폼
3. **Custom HBM** — HBM5 또는 C-HBM4e; NVIDIA GTC 2026에서 "custom HBM" 확인
4. **고급 3D 스태킹** — Backside Power Delivery (TSMC Super Power Rail); 패키지당 GPU 다이 8개 (Rubin Ultra 2×)
5. **Rosa CPU** — Vera CPU 후속; Feynman 수퍼칩에 탑재
6. **LP40 LPU** (Groq Gen2) — NVLink 연동, 메모리 코히런시
7. **BlueField-5 DPU**, NVSwitch 8, ConnectX-10 (3.2 Tb/s), Spectrum-7 (204 Tb/s)

---

## 세대별 통합 비교표

### GPU 단위 스펙

| 세대 | 모델 | 출시 | 메모리 타입 | 용량/GPU | 대역폭/GPU | 세대 대비 BW 향상 |
|------|------|------|------------|---------|-----------|----------------|
| G1 | H100 SXM5 | 2022 | HBM3 | 80 GB | 3.35 TB/s | — (기준) |
| G1 | H200 SXM | 2024 | HBM3e | 141 GB | 4.8 TB/s | 1.43× vs H100 |
| G2 | B200 / GB200 | 2025 | HBM3e | 192 GB | ~8 TB/s | 1.67× vs H200 |
| G2 | B300 / GB300 | 2025 | HBM3e | 288 GB | 8 TB/s | 1.0× vs B200 (용량↑) |
| G3 | R200 / Vera Rubin | 2026 H2 | HBM4 | 288 GB | ~20.5–22 TB/s | ~2.6× vs B300 |
| G3 CPX | Rubin CPX | 2026 H2 | GDDR7 | 128 GB | ~2 TB/s | N/A (Prefill 전용) |
| G3.5 | R300 / Rubin Ultra | 2027 H2 | HBM4e | 1,024 GB | 32 TB/s | ~1.5× vs R200 |
| G4 | F400 / Feynman | 2028 | Custom HBM (HBM5) | ~400–640 GB* | ~32 TB/s* | TBD |

*F400 스펙은 추정치

### NVL 랙 단위 스펙

| 세대 | 시스템 | GPU 수 | 총 메모리 | 총 HBM BW | NVLink BW | FP4 성능 | 전력 |
|------|--------|--------|---------|---------|----------|---------|------|
| G1 | DGX H100 (8× H100) | 8 | 640 GB | 26.8 TB/s | NVLink 4.0 | ~8 PFLOPS FP8 | ~10.2 kW |
| G1 | DGX H200 (8× H200) | 8 | 1,128 GB | 38.4 TB/s | NVLink 4.0 | ~16 PFLOPS FP8 | ~10.2 kW |
| G2 | GB200 NVL72 | 72 | 13.8 TB | ~576 TB/s | 130 TB/s | 1.44 EFLOPS | ~120 kW |
| G2 | GB300 NVL72 | 72 | 20.7 TB | 576 TB/s | 130 TB/s | 1.1 EFLOPS | ~132–140 kW |
| G3 | Vera Rubin NVL72 | 72 | ~20.7 TB | ~1.4 PB/s | 260 TB/s | 3.6 EFLOPS | ~300 kW |
| G3 | Vera Rubin NVL144 CPX | 144+144 | 100 TB | 1.7 PB/s | — | 8 EFLOPS | — |
| G3.5 | Rubin Ultra NVL576 | 576 | ~576 TB | ~4.6 PB/s | 1.5 PB/s | 15 EFLOPS | ~600 kW |
| G4 | Feynman NVL576 | 576 | TBD | TBD | NVLink 8 (광학) | >>15 EFLOPS | >1.2 MW |
| G4 | Feynman NVL1152 | 1,152 | TBD | TBD | NVLink 8 광학 | TBD | TBD |

---

## Vast Data & Weka: G4 스토리지 파트너 참고

위 명확화 섹션에서 설명한 대로, 이 두 업체는 GPU 세대 스펙 제공업체가 아니라 **NVIDIA 추론 메모리 계층구조의 G4(및 G3.5) 스토리지 파트너**입니다.

### Vast Data
- NVIDIA ICMS/CMX 플랫폼 파트너 (CES/GTC 2026 발표)
- BlueField-4 DPU에서 네이티브 실행; G4 영구 KV Cache 스토리지 제공
- 800 Gb/s RDMA NVMe-oF over Spectrum-X 이더넷 지원
- NVIDIA Dynamo 분산 추론 네이티브 통합 발표

### Weka (NeuralMesh)
- NeuralMesh Axon + Augmented Memory Grid 기술로 G3.5/G4 계층 지원
- BlueField-4 기반 NeuralMesh STX (KV Cache Context Memory)
- 예상 처리량: 읽기 >320 GB/s / 쓰기 >150 GB/s
- 기존 CPU 연결 스토리지 대비 토큰/와트 >100× 개선 주장
- 다음 세대 NeuralMesh: 2026 H2 예정 (BlueField-4 가용성 기준)

---

## 참고 자료

- NVIDIA H100/H200 공식 데이터시트 및 제품 페이지
- NVIDIA GB200/GB300 NVL72 공식 페이지
- NVIDIA Rubin CPX 뉴스룸 발표 (GTC 2026)
- NVIDIA BlueField-4 ICMS 발표 (CES 2026)
- Tom's Hardware: "Nvidia's Vera Rubin Platform In-Depth" (2026)
- Data Center Dynamics: "Nvidia's Rubin Ultra NVL576 rack expected to be 600kW" (2026)
- Tom's Hardware: "Nvidia enterprise roadmap: Rubin, Rubin Ultra, Feynman" (2026)
- Tweaktown: "Nvidia updates roadmap with Feynman details" (2025)
- Tom's Hardware: "Nvidia GTC 2026 roadmap: Rosa CPU, Feynman, optical NVLink" (2026)
- The Next Platform: "Driving Down the AI System Roadmap With Nvidia" (2026.03.19)
- Blocks & Files: "Nvidia GTC storage news roundup" (2026.03.17)
- VideoCardz: "Nvidia Rubin CPX GPU to feature 128GB GDDR7 memory" (2026)
- SemiAnalysis: Rubin HBM4 달성 어려움 관련 보고
- WCCFTech: "Next-Gen HBM Architecture (HBM4–HBM8) detailed" (2025)
- VAST Data 블로그: NVIDIA Dynamo + VAST 통합
- Weka 블로그: NeuralMesh + BlueField-4

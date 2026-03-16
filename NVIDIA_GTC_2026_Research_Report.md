# NVIDIA GTC 2026 조사보고서

**일시:** 2026년 3월 16일 ~ 19일
**장소:** SAP Center, San Jose, California
**참석 규모:** 190개국 약 39,000명
**키노트:** Jensen Huang CEO, 3월 16일 오전 11시 (PT), 약 2시간

---

## 1. 메모리 관련 주요 발표 및 분석

### 1.1 HBM4: Rubin GPU의 핵심 메모리

**현황과 의미:**

Rubin GPU는 HBM4 메모리를 독점 채택한다. 이는 HBM3e에서 HBM4로의 세대 전환이 본격화됨을 의미하며, 메모리 업계 전체에 파급 효과가 크다.

| 항목 | Blackwell (B200/B300) | Rubin | 변화 |
|------|----------------------|-------|------|
| 메모리 종류 | HBM3e / HBM4 | HBM4 전용 | 세대 전환 완료 |
| GPU당 용량 | 192~288GB | 288GB | 동일~증가 |
| 대역폭 | 8 TB/s | 13~24 TB/s | **1.6~3배 증가** |
| I/O 인터페이스 | 1,024-bit | 2,048-bit | **2배 확대** |
| 핀 속도 | ~5.6 Gbps | 10~13 Gbps | **2배 이상** |

**핵심 분석 포인트:**

- **대역폭 2배 이상 도약:** HBM4는 I/O 인터페이스를 1,024-bit에서 2,048-bit으로 두 배 확대했다. 이것이 단순한 스펙 업그레이드가 아닌 이유는, AI 추론(inference)에서의 병목이 바로 "메모리 대역폭"이기 때문이다. 모델이 토큰을 하나씩 생성하는 디코드 단계에서 데이터를 충분히 빠르게 읽어오지 못하면 GPU의 연산 능력이 아무리 높아도 성능이 제한된다. HBM4의 대역폭 향상은 이 병목을 직접적으로 완화한다.

- **NVIDIA의 스펙 유연화 전략:** NVIDIA는 원래 11.7 Gbps 이상의 핀 속도를 요구했으나, 수율 문제로 인해 10 Gbps와 11 Gbps 두 가지 티어로 인증을 진행 중이다. 핀 속도를 11.7 Gbps에서 약 10 Gbps로 낮추면 스택당 대역폭이 1.5 TB/s+에서 약 1.35 TB/s로 감소하지만, 여전히 Blackwell B200의 1.2 TB/s보다 우수하다. 더 중요한 것은 **발열이 크게 감소**한다는 점으로, 이는 전력 효율이 핵심인 대규모 데이터센터에서 실질적인 이점이 된다.

- **16-Hi HBM4의 기술적 도전:** NVIDIA는 2026년 4분기까지 16층 적층 HBM4를 요구하고 있다. 현재 12층 HBM의 웨이퍼 두께가 약 50μm인데, 16층을 위해서는 약 30μm까지 얇게 해야 한다(JEDEC 패키지 높이 775μm 제한). 현재 SK hynix와 Samsung 모두 **수율이 20% 미만**으로 보고되며, 이는 공급 제약이 지속될 가능성을 시사한다.

### 1.2 HBM4 공급 전쟁: SK hynix vs Samsung vs Micron

**시장 지형 변화:**

| 업체 | 2025년 HBM 점유율 | 2026년 전망 | 주요 기술 |
|------|-------------------|-------------|----------|
| SK hynix | 59% | 50% (하락) | Advanced MR-MUF |
| Samsung | 20% | 28% (상승) | TC-NCF |
| Micron | 21% | 22% (소폭 상승) | - |

**핵심 분석 포인트:**

- **Samsung의 반격:** NVIDIA가 HBM4 스펙을 하향 조정한 것은 Samsung에 유리하게 작용했다. Samsung의 TC-NCF(Thermal Compression Non-Conductive Film) 기술은 높은 16층 적층에서 더 나은 갭 충진과 구조적 안정성을 제공하며, 스펙 완화로 인해 Samsung의 HBM4가 실질적인 경쟁력을 확보했다. Samsung은 2026년 2월부터 HBM4 출하를 시작했으며, **Vera Rubin 전용 HBM4에서는 Samsung이 리드**할 것으로 전망된다.

- **SK hynix의 과제:** SK hynix는 전체 HBM 공급의 약 60%를 확보했지만, 11 Gbps 인증을 아직 통과하지 못한 상태다. MR-MUF 기술은 HBM3e에서는 우수했으나, 16층 적층에서는 제조 복잡도와 비용이 크게 증가한다. GTC 직전 Jensen Huang이 SK hynix와 "세계 최고의 메모리 반도체 팀과의 축하 만찬"을 가진 것은 파트너십의 중요성을 보여준다.

- **Micron의 포지션:** Micron도 16-Hi 샘플을 NVIDIA에 전달했으며, 2026년 전체 HBM4 생산 용량이 이미 매진된 상태다.

- **DRAM 시장 파급:** AI 데이터센터 확장으로 메모리 시장이 전통적 수급 사이클을 벗어나 구조적 공급 부족이 심화되고 있으며, 현물가와 계약가 격차가 40~50%까지 벌어졌다. 2026년 2분기 DRAM 가격이 최대 70% 급등할 것으로 전망된다.

### 1.3 SRAM 기반 추론 칩 — Groq 인수의 함의

**배경:**

NVIDIA는 2025년 12월 24일 Groq를 200억 달러에 인수(라이선싱+인력 영입 형태)했다. Jensen Huang은 이를 2020년의 Mellanox 인수에 비유하며, "Mellanox가 NVIDIA를 네트워킹 회사로 만든 것처럼, Groq가 NVIDIA를 추론 인프라 회사로 변모시킬 것"이라 했다.

**LPU(Language Processing Unit)의 핵심 — 온칩 SRAM:**

| 항목 | GPU (H100) | Groq LPU |
|------|-----------|----------|
| 메모리 방식 | 외장 HBM (80GB) | 온칩 SRAM (230MB) |
| 내부 대역폭 | 3.35 TB/s | **80 TB/s** |
| 연산 활용률 (추론 시) | 30~40% | **~100%** |
| 토큰 생성 속도 | ~100 tok/s | **300~750 tok/s** |
| 에너지 소비 | 10~30 J/토큰 | **1~3 J/토큰** |

**핵심 분석 포인트:**

- **HBM을 대체하는 것이 아닌, 보완하는 역할:** 시장에서는 SRAM 추론 칩이 HBM 수요를 줄일 것이라는 우려로 한국 메모리 주가가 급락했으나, 전문가들은 이를 "과장된 해석"으로 본다. SRAM은 비트당 트랜지스터가 6개(DRAM은 1개)로 밀도가 낮고 비용이 높아, 대용량 메모리 대체는 비현실적이다. SRAM이 유리한 워크로드는 **80억 파라미터 이하의 소형 모델**이며, 대형 모델은 여전히 HBM이 필수다.

- **그러나 무시할 수 없는 시장:** 소형 모델 + 초저지연이 필요한 엣지 추론, 로보틱스, 음성, IoT 분야는 "지금까지 NVIDIA가 서비스하지 못했던 거대한 시장"이다. 한 LPU에 230MB SRAM만 있으므로 70B 모델을 돌리려면 576개의 LPU가 필요하지만, 8B 이하 모델에서는 GPU 대비 압도적인 지연시간과 에너지 효율을 제공한다.

- **LPX 랙 아키텍처:** NVIDIA는 LPU 기반의 새로운 랙 아키텍처 "LPX"를 공개 예정이다. 초기에는 64개 LPU(32개 RealScale ASIC 타일), GTC 2026에서 **256개 LPU 랙**을 소개할 예정이다. 52층 M9 Q-glass PCB 사용, 액체 냉각 채택 등 고밀도 설계가 특징이다.

- **3계층 추론 인프라의 탄생:** NVIDIA는 추론 워크로드를 3개 계층으로 분리하는 전략을 취한다:
  - **Vera Rubin (HBM4):** 대규모 모델의 장문맥 추론
  - **LPX (SRAM/LPU):** 초저지연 실시간 추론 (소형 모델)
  - **CPX (GDDR7):** 장문맥 프리필 연산

### 1.4 Inference Context Memory Storage (ICMS) — 스토리지의 AI 네이티브 전환

**이것이 중요한 이유:**

Agentic AI가 부상하면서 문맥 창(context window)이 수백만 토큰으로 확대되고 있다. 이때 핵심이 되는 것이 **KV(Key-Value) 캐시**인데, 이를 GPU 메모리에 장기 보관하면 실시간 추론의 병목이 된다. NVIDIA는 BlueField-4 DPU 기반으로 **KV 캐시를 고대역폭 플래시 스토리지 계층으로 오프로드**하는 새로운 아키텍처를 발표했다.

**ICMS 플랫폼의 핵심 기능:**

- Rubin 포드 전체에서 KV 캐시를 **공유 가능한 고대역폭 장기 메모리 자원**으로 전환
- 기존 스토리지 대비 **5배 높은 전력 효율**
- NVIDIA NIXL 라이브러리 및 Dynamo 소프트웨어와 통합하여 초당 토큰 수 극대화
- BlueField-4의 하드웨어 가속 KV 캐시 배치로 메타데이터 오버헤드 제거
- **ASTRA(Advanced Secure Trusted Resource Architecture):** 대규모 AI 인프라의 보안 프로비저닝을 위한 신뢰 아키텍처

**핵심 분석 포인트:**

- Jensen Huang이 "AI가 이제 스토리지까지 혁신하고 있다"고 언급한 것은 스토리지가 AI 인프라의 핵심 계층으로 격상되었음을 의미한다. 단순한 데이터 저장소가 아니라, **추론 성능에 직접 영향을 미치는 "컨텍스트 메모리"**로서의 역할이 강조된다.
- 파트너사로 **VAST Data**와 **WEKA**가 참여하며, 2026년 하반기 출시 예정이다. VAST Data는 BlueField-4 DPU 위에서 네이티브로 실행되는 새로운 추론 아키텍처를 발표했다.
- 멀티 에이전트 시스템에서 다수의 AI 에이전트가 동일한 KV 캐시를 공유할 수 있게 됨으로써, 재연산 없이 문맥을 유지하는 것이 가능해진다. 이는 에이전틱 AI의 실용화에 필수적인 인프라다.

---

## 2. NVIDIA 로드맵 분석

### 2.1 현재~2027: Rubin 플랫폼

**Rubin 핵심 사양:**

| 항목 | 사양 |
|------|------|
| 공정 | TSMC 3nm |
| 트랜지스터 | 3,360억 개 |
| 메모리 | HBM4, 최대 288GB/GPU |
| 대역폭 | 최대 24 TB/s |
| FP4 성능 | 50 PFLOPS (Blackwell 20 PFLOPS의 2.5배) |
| NVLink 6 | 총 260 TB/s (2배 향상) |
| 랙 성능 (NVL144) | 3.6 EFLOPS FP4 (Blackwell NVL72 1.1 EFLOPS의 3.3배) |

**Rubin이 가져올 실질적 변화:**

- Blackwell 대비 **추론 토큰 비용 10배 절감**
- MoE 모델 학습에 필요한 **GPU 수 4배 감소**
- 2026년 하반기 AWS, Google Cloud, Microsoft Azure, OCI 등에서 배포 시작
- Vera CPU + Rubin GPU + NVLink 6 Switch + ConnectX-9 SuperNIC + BlueField-4 DPU + Spectrum-6 Switch의 **6칩 극한 코디자인**

### 2.2 2027: Rubin Ultra

- Rubin 코어 2개를 연결한 형태
- **FP4 성능 100 PFLOPS** (Rubin의 2배)
- **HBM4e** 메모리 채택 (HBM4의 개량형, 더 높은 대역폭)
- Vera Rubin Ultra NVL576 예상 — 576개 GPU

### 2.3 2028: Feynman 아키텍처 (차세대)

| 항목 | 내용 |
|------|------|
| 공정 | TSMC A16 (1.6nm) — **2nm 노드를 건너뛰는 전략** |
| 핵심 혁신 | **실리콘 포토닉스** 최초 도입 |
| 메모리 | "차세대 HBM" (HBM4e 또는 그 이후) |
| 인터커넥트 | 8세대 NVSwitch, Spectrum 7 Ethernet, CX10 InfiniBand 광학 |
| 설계 방향 | "Inference-First" 아키텍처 |
| TDP | 1,000W 초과 예상 |
| 패키징 | Intel 14A/18A EMIB 활용 가능성 |

**핵심 분석 포인트:**

- **실리콘 포토닉스의 도입이 가장 주목할 변화다.** 전기 신호 대신 광신호로 데이터를 전송하여 극한의 대역폭을 낮은 전력으로 구현한다. NVIDIA는 Ayar Labs 등에 투자하며 이 기술을 준비해왔다. 칩 내부 포토닉스인지 랙 간 인터커넥트인지는 미확인이나, 어느 쪽이든 데이터 이동의 에너지 효율을 혁신적으로 개선할 수 있다.
- **2nm을 건너뛰고 1.6nm으로 직행**하면 경쟁사 대비 2~3년의 기술 격차를 확보할 수 있다.
- Feynman이 "추론 우선(Inference-First)" 설계라는 것은, 에이전틱 AI 시대에 추론 워크로드가 학습을 압도할 것이라는 NVIDIA의 판단을 반영한다.

### 2.4 연간 로드맵 요약

```
2026 H2  ──  Rubin (3nm, HBM4, 50 PFLOPS FP4)
   │
2027     ──  Rubin Ultra (HBM4e, 100 PFLOPS FP4)
   │
2028     ──  Feynman (1.6nm, 실리콘 포토닉스, 차세대 HBM)
   │
2029+    ──  Feynman Ultra (예상)
```

---

## 3. 그 외 주요 발표 사항

### 3.1 LPX + Groq LPU 추론 플랫폼

- OpenAI가 첫 번째 주요 고객으로 3GW 규모의 전용 용량 배정
- GTC에서 Jonathan Ross(전 Groq CEO, 현 NVIDIA)가 "GPU ♥ LPU" 세션 발표
- GPU는 학습 + 대형 모델 추론, LPU는 소형 모델 초저지연 추론으로 역할 분담

### 3.2 N1/N1X 노트북 CPU

- Arm 아키텍처 기반 Windows 노트북용 프로세서
- Qualcomm과 경쟁하되 게이밍에 특화
- The Verge에서 유출된 정보로, 공식 발표 가능성 있음

### 3.3 NemoClaw — 오픈소스 엔터프라이즈 AI 에이전트 플랫폼

- 기업이 AI 에이전트를 구조적으로 구축하고 배포할 수 있는 오픈소스 플랫폼
- 에이전틱 AI 생태계 확대 전략의 일환

### 3.4 NVLink Fusion 확장

- SiFive가 RISC-V CPU에 NVLink Fusion을 통합하는 계약 체결
- NVLink가 NVIDIA GPU 전용에서 범용 고대역폭 인터커넥트로 확장되는 신호
- CXL과의 경쟁에서 NVLink를 업계 표준으로 밀어붙이려는 의도

### 3.5 CXL 생태계 동향

- NVIDIA 자체는 CXL보다 NVLink를 우선시하나, 파트너 생태계에서는 CXL 활용 확대
- **LIQID:** GPU와 CXL 메모리 풀링/공유 솔루션을 GTC에서 시연. 업계 유일의 멀티패브릭 플랫폼으로 GPU와 CXL 메모리를 실시간 동적 풀링
- NVIDIA GPU의 HBM에 대한 CXL 액세스는 여전히 불가 — NVIDIA는 PCIe 같은 표준 인터커넥트보다 대역폭을 우선시

### 3.6 주요 파트너십

| 파트너 | 내용 |
|--------|------|
| Microsoft | Vera Rubin NVL72 랙 시스템을 차세대 AI 데이터센터에 배포 |
| Meta | 새 AI 데이터센터를 NVIDIA 프로세서로 채우는 다년 계약 |
| Thinking Machines Lab | 1GW 이상의 Vera Rubin 시스템 배포를 위한 다년 파트너십 |
| AWS, Google, OCI | 2026년 Rubin 인스턴스 최초 배포 예정 |

---

## 4. 메모리/스토리지 관점 핵심 시사점 종합

1. **HBM4는 단기 공급 부족이 불가피하다.** 16-Hi 수율 20% 미만, 3사 경쟁 심화, NVIDIA의 스펙 유연화까지 고려하면 HBM4는 2026~2027년 가장 전략적인 반도체 부품이 될 것이다.

2. **SRAM은 HBM의 적이 아니라 새로운 시장을 여는 열쇠다.** Groq LPU의 SRAM 아키텍처는 HBM을 대체하는 것이 아니라, 지금까지 GPU가 커버하지 못한 초저지연 추론 시장을 개척한다. 메모리 업계에 대한 위협보다는 추론 시장 전체 파이의 확대로 해석해야 한다.

3. **스토리지가 AI 추론 성능의 핵심 변수로 부상했다.** BlueField-4 기반 ICMS는 KV 캐시를 플래시 스토리지로 오프로드함으로써, 스토리지를 단순 저장소에서 "컨텍스트 메모리"로 격상시켰다. 에이전틱 AI 시대에 스토리지 인프라의 가치가 재평가될 것이다.

4. **NVLink vs CXL 경쟁이 인터커넥트 전략의 핵심이다.** NVIDIA는 NVLink Fusion을 RISC-V 등 비-NVIDIA 플랫폼으로 확장하며, CXL 대비 대역폭 우위를 통해 생태계 록인을 강화하고 있다. CXL은 파트너 수준에서의 메모리 풀링 용도로 제한적 역할을 할 전망이다.

5. **2028년 Feynman의 실리콘 포토닉스가 메모리/인터커넥트 패러다임을 바꿀 수 있다.** 광학 인터커넥트가 실현되면 데이터 이동의 에너지 비용이 극적으로 줄어들며, HBM과 GPU 간의 대역폭 병목도 새로운 방식으로 해결될 수 있다.

---

---

## 5. 주제별 스토리지 요구사항 상세 분석

### 5.1 Vera Rubin NVL72 — 학습(Training) 워크로드

#### 5.1.1 메모리 계층 구조

```
┌─────────────────────────────────────────────────────────────────────┐
│                    Vera Rubin NVL72 메모리 계층                      │
├──────────┬──────────┬──────────┬─────────────┬──────────────────────┤
│  계층    │ 매체     │ GPU당 용량│ 대역폭      │ 지연시간             │
├──────────┼──────────┼──────────┼─────────────┼──────────────────────┤
│ G1 (L1)  │ 레지스터 │ ~수 MB   │ ~수십 TB/s  │ <1 ns               │
│ G2 (L2)  │ On-chip  │ ~수십 MB │ ~수 TB/s    │ ~수 ns               │
│          │ SRAM     │          │             │                      │
│ G3 (HBM) │ HBM4    │ 288 GB   │ 13~22 TB/s  │ ~100 ns              │
│ G3.5     │ NVMe    │ 16 TB    │ 수십 GB/s   │ ~10 μs               │
│ (ICMS)   │ Flash    │ (GPU당)  │ (RDMA)      │ (RDMA 최적화)        │
│ G4       │ 네트워크 │ PB급     │ 수 GB/s     │ ~ms                  │
│          │ 스토리지 │          │             │                      │
└──────────┴──────────┴──────────┴─────────────┴──────────────────────┘
```

#### 5.1.2 학습 데이터 스토리지

| 항목 | 요구사항 | 근거 |
|------|---------|------|
| **데이터셋 용량** | 수십 PB ~ 수백 PB | 1T+ 파라미터 모델 학습 시 토큰 수 10T+, 멀티모달 데이터 포함 |
| **읽기 처리량** | 100+ GB/s (클러스터 전체) | GPU idle 방지를 위해 데이터 파이프라인이 연산 속도를 따라가야 함 |
| **IOPS** | 수백만 IOPS | 소규모 랜덤 읽기 패턴 (데이터 셔플링, 증강) |
| **매체** | NVMe SSD (3+ DWPD) | SATA 대비 3~7배 처리량, 10~100배 낮은 지연시간 |
| **아키텍처** | 분산 병렬 파일시스템 | Lustre, GPFS, WekaFS 등 |

**워크로드 특징:**
- **순차 읽기 중심:** 학습 데이터는 에포크 단위로 전체를 순차적으로 읽음
- **대역폭 바운드:** 높은 처리량이 핵심이며, 지연시간은 상대적으로 덜 중요
- **데이터 불변성:** 학습 중 데이터셋은 읽기 전용 (쓰기 없음)
- **프리페치 가능:** 다음 배치를 미리 로드하여 GPU 대기 시간 최소화

#### 5.1.3 체크포인트 스토리지

| 항목 | 요구사항 | 근거 |
|------|---------|------|
| **단일 체크포인트 크기** | 수 TB ~ 15+ TB | 1T 파라미터 모델: 가중치(~2TB FP16) + 옵티마이저 상태(~8TB) + 그래디언트 |
| **쓰기 처리량** | 100+ GBps (버스트) | 512-GPU 클러스터에서 15TB 체크포인트를 150초 이내에 기록 |
| **빈도** | 수 분 ~ 수 시간 간격 | 빈번할수록 장애 복구 비용 감소, 그러나 I/O 오버헤드 증가 |
| **보존 기간** | 로컬: 수 시간 / 영구: 무기한 | 2계층 전략 — 핫(NVMe) + 콜드(S3/오브젝트 스토리지) |
| **총 용량** | 수백 TB ~ PB급 | 다수 체크포인트 버전 보존 필요 |

**워크로드 특징:**
- **극한 버스트 쓰기:** 체크포인트 기록 시 클러스터 전체가 동시에 수 TB를 기록 → 스토리지 버스트 대역폭이 핵심
- **3D 병렬 쓰기 패턴:** 수천 GPU가 각각 독립 파일을 생성 → 메타데이터 병목 발생 가능
- **2계층 비동기 전략:** 로컬 NVMe에 즉시 기록 → 백그라운드에서 내구성 스토리지로 복제
- **GPU stall 비용이 거대:** 512-GPU 클러스터가 체크포인트 I/O로 1분 멈추면, 클라우드 비용 기준 수만 달러 손실

```
체크포인트 쓰기 흐름:

GPU HBM ──(PCIe 5.0)──▶ 호스트 DRAM ──(NVMe)──▶ 로컬 SSD (핫)
                                                       │
                                                 (백그라운드)
                                                       ▼
                                              영구 스토리지 (S3/Lustre)
```

---

### 5.2 Vera Rubin NVL72 — 추론(Inference) 워크로드

#### 5.2.1 KV 캐시 스토리지 — 가장 급격히 성장하는 스토리지 수요

**KV 캐시란?**

LLM이 토큰을 생성할 때마다 이전 토큰들의 Key-Value 쌍을 저장하여 재연산을 피한다. 문맥 길이에 비례하여 선형 성장하며, 동시 사용자 수에 비례하여 곱셈적으로 증가한다.

**규모 산정:**

| 모델 규모 | 문맥 길이 | KV 캐시 크기 (세션당) | 비고 |
|----------|----------|---------------------|------|
| 8B | 100K 토큰 | ~10 GB | 단일 GPU HBM 내 수용 가능 |
| 70B | 100K 토큰 | ~40 GB | HBM 용량의 상당 부분 소비 |
| 405B | 1M 토큰 | ~200+ GB | 다수 GPU의 HBM 소비 또는 오프로드 필수 |
| 1T+ | 1M+ 토큰 | ~500+ GB | ICMS 오프로드 없이는 운영 불가 |

**에이전틱 AI에서의 폭발적 증가:**

- 에이전트 1회 호출 시 공유 컨텍스트: 시스템 프롬프트(~2,000 토큰) + 도구 정의(~5,000 토큰) + 정책 컨텍스트(~4,000 토큰) = **~11,000 토큰**
- 하루 5,000회 에이전트 호출 시: **5,500만 토큰의 공유 컨텍스트** 재연산
- KV 캐시 미스 비용은 히트 비용의 **10배** (Manus AI 보고)
- 따라서 "KV 캐시 히트율"이 에이전틱 AI의 **가장 중요한 성능 지표**

#### 5.2.2 ICMS (Inference Context Memory Storage) 플랫폼 상세

**NVIDIA가 정의한 "G3.5" 계층:**

GPU HBM(G3)과 네트워크 스토리지(G4) 사이에 위치하는 **팟 수준의 컨텍스트 메모리 계층**이다.

| 항목 | 사양 |
|------|------|
| **GPU당 컨텍스트 메모리** | 16 TB |
| **랙당 용량** | 72 GPU × 16 TB = **1,152 TB** |
| **SuperPOD 총 용량** | **18,432 TB (~18 PB)** |
| **스토리지 인클로저 구성** | 4× BlueField-4 DPU + 600 TB 플래시 / 인클로저 |
| **인클로저 수 (SuperPOD)** | 36× 2U 인클로저 |
| **네트워크** | Spectrum-X Ethernet, 800 Gb/s RDMA |
| **프로토콜** | NVMe-oF (NVMe over Fabrics) |
| **성능 향상** | 토큰/초 5배, 전력 효율 5배 (전통 스토리지 대비) |

**BlueField-4 DPU의 역할:**

| 기능 | 상세 |
|------|------|
| **KV 캐시 I/O 플레인** | NVMe-oF 및 RDMA 프로토콜을 라인 레이트로 처리 |
| **하드웨어 가속 KV 배치** | 메타데이터 오버헤드 제거, 호스트 CPU 개입 없이 DPU가 직접 NVMe 디바이스 접근 |
| **인라인 암호화** | 800 Gb/s 대역폭에서 CPU 사이클 소비 없이 암호화/무결성 검증 |
| **프로세서** | 64코어 NVIDIA Grace CPU + 고대역폭 LPDDR |
| **보안** | ASTRA (Advanced Secure Trusted Resource Architecture) |

**워크로드 특징:**
- **임시적(Ephemeral) 데이터:** KV 캐시는 영구 저장이 아닌 재연산 가능한 임시 데이터 → 내구성보다 성능과 용량 우선
- **읽기/쓰기 혼합:** 프리필 시 대량 쓰기 → 디코드 시 반복 읽기 → 세션 종료 시 삭제
- **공유 접근:** 여러 GPU가 동일 KV 캐시를 참조 (멀티 에이전트 시스템)
- **지연시간 민감:** ms 단위 접근 필요 (기존 엔터프라이즈 스토리지의 ms 지연은 추론 stall 유발)
- **NAND 플래시에 최적:** 대용량 + 중간 대역폭 + 중간 지연시간 요구 → DRAM보다 경제적, HDD보다 빠름

```
ICMS 데이터 흐름:

사용자 요청 ──▶ 프리필 (CPX) ──▶ KV 캐시 생성 ──▶ ICMS에 저장
                                                       │
다음 요청 ──▶ ICMS에서 KV 캐시 로드 ──▶ 디코드 (Rubin GPU)
                    │
                (캐시 히트 시 프리필 생략 → 10배 비용 절감)
```

---

### 5.3 Rubin CPX — 장문맥 프리필 전용

#### 5.3.1 아키텍처적 차별점

CPX는 **프리필(Prefill) 전용** GPU로, HBM 대신 GDDR7을 채택한 최초의 CUDA GPU이다.

| 항목 | Rubin (R200) | Rubin CPX | 비교 |
|------|-------------|-----------|------|
| **메모리 종류** | HBM4 | GDDR7 | 비용 1/5 |
| **GPU당 용량** | 288 GB | 128 GB | 절반 이하 |
| **대역폭** | 13~22 TB/s | 2 TB/s | 1/7~1/11 |
| **연산 성능** | 50 PFLOPS FP4 | 30 PFLOPS FP4 | 60% |
| **Attention 가속** | 기본 | 3배 가속 (vs GB300) | 프리필 최적화 |
| **비디오 코덱** | 없음 | HW 인코더/디코더 내장 | 멀티모달 지원 |

#### 5.3.2 스토리지 요구사항

| 항목 | 요구사항 | 근거 |
|------|---------|------|
| **모델 가중치 로딩** | 128 GB GDDR7 내 수용 | MoE 모델은 활성 파라미터만 로드하므로 GDDR7으로 충분 |
| **KV 캐시 출력** | 대량 쓰기 → ICMS로 전송 | 프리필 결과를 ICMS에 기록, 디코드 GPU가 읽음 |
| **입력 데이터** | 장문맥 텍스트/멀티모달 데이터 | 100만+ 토큰, 비디오/이미지 인코딩 포함 |
| **네트워크 대역폭** | NVLink 6 (260 TB/s 총합) | KV 캐시를 디코드 GPU로 전송하는 경로 |

**워크로드 특징:**
- **연산 바운드(Compute-Bound):** 프리필은 모든 입력 토큰을 한 번에 처리 → GPU 연산 능력이 병목이지 메모리 대역폭이 아님
- **대역폭 요구 낮음:** HBM의 1/7 대역폭(2 TB/s)으로도 충분 → GDDR7로 비용 80% 절감
- **대량 KV 캐시 생성:** 100만 토큰 프리필 시 수백 GB의 KV 캐시 생성 → 이를 ICMS에 빠르게 기록해야 함
- **일회성 읽기:** 입력 데이터를 한 번만 읽고 처리 → 캐싱 전략보다 스트리밍 처리량이 중요

```
CPX 프리필 워크로드 흐름:

장문맥 입력 ──▶ [CPX: 128GB GDDR7] ──▶ KV 캐시 생성
(100만+ 토큰)    (30 PFLOPS FP4)         (수백 GB)
                                           │
                                     (NVLink 6 / ICMS)
                                           ▼
                                   [Rubin GPU: HBM4]
                                    디코드 (토큰 생성)
```

---

### 5.4 LPX (Groq LPU) — 초저지연 추론

#### 5.4.1 스토리지 아키텍처의 근본적 차이

LPU는 **외장 메모리가 전혀 없다.** 모든 모델 가중치가 온칩 SRAM에 저장된다.

| 항목 | GPU (H100) | LPU (Groq) | 차이 |
|------|-----------|-----------|------|
| **주 메모리** | HBM3 80GB (외장) | SRAM 230MB (온칩) | 용량 348배 차이 |
| **내부 대역폭** | 3.35 TB/s | 80 TB/s | LPU 24배 우위 |
| **메모리 접근 에너지** | 높음 (HBM 접근) | 낮음 (온칩 SRAM) | 에너지 10배 절감 |
| **외부 스토리지 의존** | 모델 로딩 시 필요 | 모델 분산 로딩 필요 | 둘 다 필요하나 패턴 다름 |

#### 5.4.2 모델 크기별 LPU 요구량과 스토리지 연계

| 모델 크기 | 정밀도 | 가중치 크기 | 필요 LPU 수 | 모델 로딩 소스 |
|----------|--------|-----------|-----------|--------------|
| 1B | FP16 | 2 GB | ~9개 | 로컬 NVMe에서 로드 |
| 7B | FP16 | 14 GB | ~61개 | 로컬 NVMe에서 로드 |
| 8B | INT8 | 8 GB | ~35개 | 로컬 NVMe에서 로드 |
| 70B | FP16 | 140 GB | ~576개 | 분산 스토리지에서 병렬 로드 |
| 405B | FP16 | 810 GB | ~3,522개 | 대규모 분산 스토리지 필수 |

#### 5.4.3 스토리지 요구사항

| 항목 | 요구사항 | 근거 |
|------|---------|------|
| **모델 가중치 저장** | 로컬 NVMe 또는 분산 스토리지 | 수백~수천 LPU에 가중치를 분산 로드해야 함 |
| **모델 로딩 속도** | 초고속 필요 (수 초 이내) | 모델 교체(hot-swap) 시 서비스 중단 최소화 |
| **KV 캐시** | 온칩 SRAM 내에서 처리 | 외부 KV 캐시 오프로드 불필요 (소형 모델이므로) |
| **컨텍스트 길이 제한** | 수천~수만 토큰 | SRAM 용량 제약으로 장문맥 불가 |
| **양자화 모델 저장** | INT8/FP8 양자화 모델 | TruePoint Numerics로 2~4배 용량 절감 |

**워크로드 특징:**
- **스토리지 접근이 극히 드묾:** 모델 로딩 후에는 외부 스토리지 접근이 거의 없음
- **모델 로딩이 유일한 I/O 이벤트:** 가중치를 SRAM에 로드하면, 이후 추론은 100% 온칩에서 완결
- **양자화가 스토리지 부담을 줄임:** FP16 → INT8 전환으로 저장 용량 절반, LPU 수도 절반으로 감소
- **콜드 스타트 문제:** 수백~수천 LPU에 모델을 분산 로드하는 시간이 서비스 가용성의 핵심 변수
- **멀티 모델 서빙:** 여러 모델을 빠르게 교체하려면 스토리지에서의 모델 로딩 속도가 중요

```
LPU 모델 로딩 vs 추론:

[로딩 단계] ── 외부 NVMe ──(분산)──▶ LPU#1 SRAM (230MB 슬라이스)
                               ──▶ LPU#2 SRAM (230MB 슬라이스)
                               ──▶ ...
                               ──▶ LPU#576 SRAM (230MB 슬라이스)

[추론 단계] ── 입력 토큰 ──▶ LPU 체인 (순차 처리) ──▶ 출력 토큰
                          (외부 스토리지 접근 없음)
                          (80 TB/s 내부 대역폭)
```

---

### 5.5 학습 vs 추론 스토리지 요구사항 비교표

| 특성 | 학습 (Training) | 추론 — 디코드 (Inference) | 추론 — 프리필 (Prefill) | LPX 추론 (LPU) |
|------|----------------|------------------------|----------------------|----------------|
| **주 메모리** | HBM4 (288GB) | HBM4 (288GB) | GDDR7 (128GB) | SRAM (230MB×N) |
| **데이터 패턴** | 순차 읽기 + 버스트 쓰기 | 랜덤 읽기/쓰기 혼합 | 순차 읽기 → 대량 쓰기 | 로딩 시에만 순차 읽기 |
| **외부 스토리지 용량** | 수십~수백 PB | 16 TB/GPU (ICMS) | 입력 데이터 크기 의존 | 모델 저장소 수 TB |
| **대역폭 요구** | 100+ GB/s (지속) | 수십 GB/s (RDMA) | 중간 (연산 바운드) | 로딩 시에만 높음 |
| **지연시간 민감도** | 낮음 (프리페치 가능) | **매우 높음** (μs 단위) | 중간 | N/A (온칩 처리) |
| **데이터 지속성** | 영구 (체크포인트) | 임시 (KV 캐시) | 임시 (KV 캐시) | 없음 (SRAM 휘발) |
| **SSD 내구성 요구** | 3+ DWPD (높음) | 1~3 DWPD (중간) | 낮음 (읽기 중심) | 낮음 |
| **확장 단위** | 클러스터 전체 | 팟(Pod) 단위 | 랙 단위 | 모델 크기 비례 |

---

### 5.6 NAND 플래시 / SSD 시장에 대한 함의

#### 5.6.1 수요 폭발 포인트

| 드라이버 | GPU당 SSD 용량 | 근거 |
|----------|--------------|------|
| **ICMS (KV 캐시)** | **16 TB / GPU** | Jensen Huang 키노트에서 직접 언급 |
| **체크포인트** | 2~4 TB / GPU (분산) | 15TB 체크포인트 ÷ 수백 GPU |
| **학습 데이터 캐시** | 4~8 TB / 서버 | 로컬 NVMe 캐시 계층 |
| **합계** | **~20+ TB / GPU** | ICMS가 지배적 |

**산술적 의미:**
- Vera Rubin NVL72 한 랙: 72 GPU × 16 TB = **1,152 TB의 ICMS 플래시**
- SuperPOD (36 인클로저): **~18 PB의 플래시**
- 1GW 규모 AI 팩토리: 수만 GPU → **수백 PB ~ EB급 플래시 수요**

#### 5.6.2 SSD 특성 요구사항

| 특성 | ICMS (KV 캐시) 용 | 체크포인트 용 | 학습 데이터 용 |
|------|-----------------|-------------|--------------|
| **용량** | 15.36~30.72 TB | 7.68~15.36 TB | 15.36~30.72 TB |
| **인터페이스** | NVMe Gen5/Gen6 | NVMe Gen5 | NVMe Gen5 |
| **순차 읽기** | 14+ GB/s | 7+ GB/s | 14+ GB/s |
| **순차 쓰기** | 10+ GB/s | 10+ GB/s (버스트) | 낮음 |
| **내구성 (DWPD)** | 1~3 DWPD | 3+ DWPD | 0.5~1 DWPD |
| **프로토콜** | NVMe-oF (RDMA) | 로컬 NVMe | Lustre/GPFS |
| **폼팩터** | E1.S / E3.S | U.2 / E1.S | U.2 / E1.S |
| **핵심 차별점** | 일관된 저지연 QoS | 버스트 쓰기 내구성 | 순차 읽기 처리량 |

#### 5.6.3 시장 전망

1. **ICMS가 AI SSD 수요의 게임 체인저다.** GPU당 16TB라는 수치는 전례 없는 규모이며, 이는 기존의 학습 데이터 저장 수요를 압도한다. NAND 플래시 업계는 이 수요를 충족하기 위해 QLC/PLC SSD의 대용량화와 NVMe-oF 최적화에 주력해야 한다.

2. **엔터프라이즈 SSD의 요구사항이 분화된다.** KV 캐시용(저지연 QoS), 체크포인트용(버스트 쓰기 내구성), 데이터용(순차 읽기 처리량)으로 워크로드가 명확히 분리되어, 단일 SSD 제품으로 모든 요구를 충족하기 어려워진다.

3. **NVMe-oF가 AI 스토리지의 표준 프로토콜로 자리잡는다.** BlueField-4 DPU가 NVMe-oF를 라인 레이트로 처리함으로써, 호스트 CPU 오버헤드 없이 GPU에서 원격 플래시에 직접 접근하는 아키텍처가 일반화된다.

4. **플래시 가격 상승 압력.** AI 데이터센터의 플래시 수요 급증(GPU당 20TB+)과 DRAM의 AI 전환에 의한 NAND 투자 축소가 겹치면서, 2026~2027년 NAND 플래시 가격 상승이 예상된다.

---

## 출처

- [NVIDIA GTC 2026 공식 사이트](https://www.nvidia.com/gtc/)
- [NVIDIA Blog - GTC 2026 Live Updates](https://blogs.nvidia.com/blog/gtc-2026-news/)
- [NVIDIA Newsroom - Rubin Platform](https://nvidianews.nvidia.com/news/rubin-platform-ai-supercomputer)
- [NVIDIA Newsroom - BlueField-4 ICMS](https://nvidianews.nvidia.com/news/nvidia-bluefield-4-powers-new-class-of-ai-native-storage-infrastructure-for-the-next-frontier-of-ai)
- [NVIDIA Developer Blog - BlueField-4 ICMS Technical](https://developer.nvidia.com/blog/introducing-nvidia-bluefield-4-powered-inference-context-memory-storage-platform-for-the-next-frontier-of-ai/)
- [Yahoo Finance - GTC 2026 Preview](https://finance.yahoo.com/news/nvidia-gtc-2026-what-to-expect-from-nvidias-biggest-event-of-the-year-132234592.html)
- [Tom's Hardware - Rubin/Feynman Roadmap](https://www.tomshardware.com/tech-industry/semiconductors/nvidia-enterprise-roadmap-rubin-rubin-ultra-feynman-and-silicon-photonics)
- [TrendForce - Samsung/SK hynix HBM4 Supply](https://www.trendforce.com/news/2026/03/09/news-samsung-sk%E2%80%AFhynix-reportedly-tapped-as-nvidia-rubin-hbm4-suppliers-shipments-could-start-in-march/)
- [TrendForce - NVIDIA HBM4 Spec Relaxation](https://www.trendforce.com/news/2026/02/13/news-nvidia-may-relax-hbm4-specs-as-samsung-and-sk-hynix-reportedly-face-capacity-yield-limits/)
- [Bitget - SRAM Inference Chip HBM Impact](https://www.bitget.com/academy/nvidia-gtc-2026-nvidia-stock-sram-inference-chip-hbm-dram-impact)
- [Vik's Newsletter - SRAM Decode Hardware Implications](https://www.viksnewsletter.com/p/gtc-2026-preview-implications-of-sram-decode)
- [VentureBeat - GPU Era Ending](https://venturebeat.com/infrastructure/inference-is-splitting-in-two-nvidias-usd20b-groq-bet-explains-its-next-act/)
- [Korea Herald - New Nvidia AI Chip HBM Demand](https://www.koreaherald.com/article/10694454)
- [Digitimes - DRAM Price Surge](https://www.digitimes.com/news/a20260303PD201/dram-hbm-expansion-nvidia-gtc-2026.html)
- [TweakTown - SK hynix HBM4 Showcase](https://www.tweaktown.com/news/109572/sk-hynix-showcases-next-gen-48gb-hbm4-at-11-7gbps-socamm2-lpddr6-for-ai-platforms/index.html)
- [TweakTown - 16-Hi HBM4 Supply Race](https://www.tweaktown.com/news/109495/sk-hynix-samsung-and-micron-fighting-for-nvidia-supply-contracts-for-new-16-hi-hbm4-orders/index.html)
- [TSPA Semiconductor - LPX, CPO, Rubin](https://tspasemiconductor.substack.com/p/gtc-2026-outlook-how-nvidia-is-redefining)
- [IntuitionLabs - Groq Acquisition Analysis](https://intuitionlabs.ai/articles/nvidia-groq-ai-inference-deal)
- [PBX Science - Feynman Architecture](https://pbxscience.com/nvidias-feynman-architecture-what-we-actually-know-ahead-of-gtc-2026/)
- [LIQID - CXL Memory Pooling at GTC](https://finance.yahoo.com/news/liqid-demonstrate-gpu-cxl-memory-170200758.html)
- [TechCrunch - GTC 2026 Keynote](https://techcrunch.com/2026/03/12/how-to-watch-jensen-huangs-nvidia-gtc-2026-keynote/)
- [The Register - GTC 2026 Preview](https://www.theregister.com/2026/03/13/nvidia_gtc_2026_preview_tobias_mann_register/)
- [NVIDIA Developer Blog - Rubin CPX Long-Context Inference](https://developer.nvidia.com/blog/nvidia-rubin-cpx-accelerates-inference-performance-and-efficiency-for-1m-token-context-workloads/)
- [NVIDIA Developer Blog - BlueField-4 ICMS](https://developer.nvidia.com/blog/introducing-nvidia-bluefield-4-powered-inference-context-memory-storage-platform-for-the-next-frontier-of-ai/)
- [NVIDIA Newsroom - BlueField-4 AI-Native Storage](https://nvidianews.nvidia.com/news/nvidia-bluefield-4-powers-new-class-of-ai-native-storage-infrastructure-for-the-next-frontier-of-ai)
- [NVIDIA Developer Blog - Vera Rubin Platform](https://developer.nvidia.com/blog/inside-the-nvidia-rubin-platform-six-new-chips-one-ai-supercomputer/)
- [TechTarget - NVIDIA KV Cache Enterprise Storage](https://www.techtarget.com/searchStorage/news/366637161/Nvidias-new-KV-cache-makes-waves-in-enterprise-storage)
- [Blocks and Files - NVIDIA KV Cache NVMe SSD](https://blocksandfiles.com/2026/01/06/nvidia-standardizes-gpu-cluster-kv-cache-offload-to-nvme-ssds/)
- [VAST Data - NVIDIA Inference Partnership](https://www.vastdata.com/blog/more-inference-less-infrastructure-vast-nvidia)
- [Vik's Newsletter - Context Memory Storage Tokenomics](https://www.viksnewsletter.com/p/context-memory-storage-tokenomics)
- [Tom's Hardware - Vera Rubin Platform In Depth](https://www.tomshardware.com/pc-components/gpus/nvidias-vera-rubin-platform-in-depth-inside-nvidias-most-complex-ai-and-hpc-platform-to-date)
- [VideoCardz - Rubin CPX 128GB GDDR7](https://videocardz.com/newz/nvidia-rubin-cpx-gpu-to-feature-128gb-gddr7-memory-launches-end-of-2026)
- [Groq - Inside the LPU Architecture](https://groq.com/blog/inside-the-lpu-deconstructing-groq-speed)
- [Computer Weekly - Storage Requirements for AI](https://www.computerweekly.com/feature/What-are-the-storage-requirements-for-AI-training-and-inference)
- [AWS - Checkpoint Storage Architecture](https://aws.amazon.com/blogs/storage/architecting-scalable-checkpoint-storage-for-large-scale-ml-training-on-aws/)

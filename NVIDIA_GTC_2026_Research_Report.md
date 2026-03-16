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

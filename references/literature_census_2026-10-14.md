# UAV/FANET 라우팅 문헌 census — 85편 기준 (2026-10-14)

이 문서는 `latency_literature_matrix.md`(45편, 2026-09-05 스냅샷)를 **대체**한다.
라이브 Notion DB를 직접 조회한 결과이며, 45편 스냅샷은 당시 기록으로만 보존한다.

- 출처: Notion DB `UAV/FANET RL 데일리 논문 분석`
  (`collection://82c87922-d396-48b4-b6b0-27d3f7217c90`)
- 조회 시점: 2026-10-14
- 총 85편 (논문 ID 1–86, 결번 1개)

## 1. 전수 집계

| 항목 | 값 |
|---|---|
| 총 논문 | 85 |
| Packet routing 범위 | 77 |
| Q-learning | 21 |
| DQN 계열 (DQN/Double/Dueling) | 19 |
| PPO | 18 |
| MARL | 23 |
| 기타(비표준 RL) | 32 |
| 알고리즘 미기재 | 3 |
| 연구 유형 Testbed | 4 |
| 연구 유형 **Field experiment** | **0** |
| 재현성 높음 | 8 |
| 재현성 **매우 높음** | **0** |
| 재현성 보통/낮음/매우 낮음 | 35 / 26 / 13 |

**해석**: "RL 포화"는 절반만 맞다. 기타+미기재 35편(41%)은 비학습·고전·최적화
계열(harmonic potential field, evolutionary game, Kalman-OLSR, fountain code,
UCB, content-centric)이며 최근 추가분에서 비중이 크다.

## 2. 전수 확인된 공백 (제목 + 선택 이유 전수 검색)

| 주제 | 85편 중 해당 논문 수 |
|---|---|
| Lyapunov drift-plus-penalty | **0** |
| regret bound 제시 | **0** |
| backpressure | **0** |
| queue **stability 보장** | **0** |
| Field experiment (실외 자율비행 검증) | **0** |

queue 자체는 9편(ID 17, 25, 40, 43, 44, 46, 66, 69, 75)에서 등장하지만 전부
**state/feature/reward 또는 game payoff**로 쓰이며, queue 안정성을 보장하는
논문은 없다.

## 3. 가장 강한 선행연구 3종과 각각의 약점

### 3.1 Harmonic potential field — ID 72 / 73 / 79 (Novelty 5, 위협 N5)

- ID 72: IEEE IoT Journal 11(11) 2024 — 확장판. ID 73: WCNC 2024 — 회의판.
  ID 79: VTC2025-Spring — global harmonic field → anchor guidance → M-hop local DDQN.
- 방식: local ETX·GPS velocity를 discrete Laplace 방정식으로 **반복 확산**해
  global-shaped potential field를 만들고, α(지연 중시)/β(무손실 우회) 규칙으로
  next-hop 선택. **RL이 아니다** — deterministic field + greedy rule.
- 강점: PHY가 현실적(SINR/BER/radio energy), 구조 단순, 400 노드 실험.
- 약점:
  - field 수렴에 약 50 iteration + HELLO 0.5 s → **iterative diffusion 메시지 비용**
    (저자가 실측하지 않음)
  - 이동 중 field staleness가 공개 질문으로 남음
  - **queue / MAC / traffic / deadline 전혀 모델링 안 됨**
  - seed/CI/통계 검정 없음, 코드 비공개, 재현성 보통

### 3.2 Anti-jamming routing "With QoS Guarantee" — ID 48 (IEEE TCOM 73(11) 2025)

- 유일하게 **해석적 보장**을 제시: cooperative/anti-jamming game의 Nash
  equilibrium으로 throughput·routing energy·utility upper bound 유도(식 14–26),
  수렴값이 bound와 2.3% 이내.
- 검증 3층: 이론 + simulation + **실외 testbed**(Raspberry Pi 4B 5대, USRP N210
  reactive jammer). 재현성 **높음** (DB 최상위).
- 핵심 한계:
  - 학습 action이 **broadcast interval + TX power**이며, next-hop은 BATMAN
    transmission-quality argmax → **next-hop 자체가 보장 대상이 아니다**
  - bound가 주어진 topology/channel과 **모델링된 reactive-Gaussian jammer**에
    조건부. 임의 adversary/distribution shift에 대한 보장이 아님
  - 목적함수에 queue/deadline 없음 (throughput − signaling − power)
  - 코드/trace/weight 비공개, 5노드 operator-moved testbed (자율비행 아님)

### 3.3 Improved UCB relay selection — ID 69 (ICECAI 2025, Novelty 2, 위협 N2)

- bandit을 쓴 유일한 라우팅 논문이지만:
  - 제목은 multi-hop이나 실제 모델은 **source → UAV 1대 → command center**
    one-relay (stateless MAB, MDP 아님)
  - **regret bound 없음**, nonstationarity 대응(SW/discounted UCB) 없음
  - PDR·queue·energy·deadline 측정 없음, 재현성 낮음

## 4. 비교 기준선으로 가장 중요한 논문

- **ID 17 Traffic-Adaptive Per-Hop Multipath Routing** — MAPPO-DM, finite queue +
  SINR 모델, on-time PDR/loss, **공개 코드 + 50 seeds**. 재현성 측면에서 이
  문헌군 최고 수준이므로 공정 비교의 기준으로 삼아야 한다.
- **ID 48** — testbed/보장/에너지 주장의 상한선.
- **ID 72 α/β** — 구조 단순·local→global 포지션의 직접 경쟁자. 반드시 동일
  simulator에 직접 구현해 비교해야 한다.

## 5. 이 census가 지지하는 연구 포지션

85편 기준으로 선행 0편인 조합:

> **1-hop 정보만 사용하고, iterative diffusion 없이, queue 안정성과
> sublinear regret을 동시에 보장하는 per-hop next-hop 규칙**

세 강자에 대한 차별점이 각각 명확하다.

| 경쟁자 | 차별점 |
|---|---|
| Harmonic field (N5) | 반복 확산/HELLO 불필요, staleness 없음, **queue 인식** (상대는 queue blind) |
| ID 48 (보장 선행) | 보장 대상이 **next-hop 자체**, 보장 종류가 비정상성 하 regret/안정성 (상대는 모델링된 jammer 하 NE bound) |
| ID 69 (bandit 선행) | 실제 multi-hop, **실제 regret bound**, 비정상성 대응 |

보조 공백: Field experiment 0/85, 재현성 "매우 높음" 0/85 → 재현 가능한
벤치마크 기여의 근거는 추측이 아니라 DB 수치다.

## 6. 미확정 항목 (`needs-verification`)

- ID 49 "Agentic AI-Empowered Reliable Routing" (IEEE TCCN 2026)는 제목에
  Agentic AI를 쓰지만 실제 기법은 HRC-QMIX(hypergraph + QMIX 계열 MARL)로
  기록되어 있다. **LLM 사용 여부는 전문 재확인 필요.** 단 "agentic routing"
  용어 자체는 이미 점유됐다고 보아야 한다.
- Lyapunov/regret 0건은 제목과 `선택 이유` 필드 전수 검색 결과다. 각 논문 본문
  부록의 보조 분석까지 확인한 것은 아니므로, 투고 전 핵심 10편은 본문 확인이
  필요하다.
- 본 census의 속성값(Novelty, 관련성, 재현성)은 DB 작성자의 screening 판정이며
  통계적 근거가 아니다.

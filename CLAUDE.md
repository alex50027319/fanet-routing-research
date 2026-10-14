# FANET Routing Research (from scratch)

동적 FANET/UAV 네트워크의 **per-packet next-hop 라우팅** 연구를 처음부터 다시
시작하는 저장소다. 선행 프로젝트 SwitchGLOBE(Global-to-Local policy distillation
+ predictive risk switching)는 **폐기**했다 — 구조가 복잡하고 SCI급 novelty를
끌어내기 어렵다고 판단했기 때문이다. SwitchGLOBE의 성능 주장·결과·방법을 이
프로젝트의 것으로 섞지 않는다.

문제 범위는 유지한다: **UAV routing**. 바뀐 것은 기법이다.

## 현재 상태 (2026-10-14)

아직 구현 코드가 없다. 지금 있는 것은 문헌 근거와 배경지식뿐이다.

- `references/literature_census_2026-10-14.md` — **현재 기준 문헌 census (85편)**
- `references/latency_literature_matrix.md` — 45편 스냅샷, **대체됨**. 현재 사실로
  인용하지 말고 latency 논문 카드 참고용으로만 쓴다
- `references/references.bib` (113 entries), `related_work_matrix.md`,
  `literature_comparison.csv`
- `knowledge/` — 구 프로젝트의 배경지식. **전부 SwitchGLOBE 전제**이므로 새
  접근법의 사실로 가정하지 않는다. `method_history.md`는 Phase 1~13의 시행착오
  기록으로 반면교사 가치가 있다
- `skill-templates/` — 구 프로젝트 SKILL.md. `run_switchglobe.py`,
  `config/switchglobe.yaml` 등 없는 경로를 참조하므로 **고치지 않고 쓰면 안 된다**

## 유력 방향 (확정 아님)

85편 census에서 선행 0편으로 확인된 포지션:

> 1-hop 정보만 사용하고, iterative diffusion 없이, **queue 안정성 + sublinear
> regret**을 동시에 보장하는 per-hop next-hop 규칙 (Lyapunov drift-plus-penalty
> + 비정상성 대응 online learning)

이건 **후보**다. 문제 정식화와 증명 가능성이 검증되기 전까지 확정된 방법으로
쓰지 않는다. 대안으로 검토했던 방향: 재현 가능한 벤치마크 기여(B),
LLM 기반 heuristic 발견(A). 판단 근거는 census 5절에 있다.

### 차별화해야 할 대상 (반드시 다뤄야 함)

| 선행연구 | 왜 중요한가 | 우리의 차별점 |
|---|---|---|
| Harmonic potential field (ID 72/73/79, N5) | 가장 강한 경쟁자. local→global shaping + 구조 단순 | 반복 확산/HELLO 불필요, staleness 없음, **queue 인식** (상대는 queue blind) |
| Anti-jamming "QoS Guarantee" (ID 48, IEEE TCOM) | 유일한 해석적 보장 + 실외 testbed, 재현성 최상 | 보장 대상이 **next-hop 자체**, 보장 종류가 비정상성 하 regret/안정성 |
| Improved UCB (ID 69) | bandit 선행 | 실제 multi-hop, 실제 regret bound, 비정상성 대응 |

### 시뮬레이터가 생기면 필수 baseline

- **Harmonic field α/β** (ID 72) — 동일 simulator에 직접 구현. 선택이 아니라 필수
- **ID 17 Traffic-Adaptive Per-Hop Multipath** — 공개 코드 + 50 seeds, 이 문헌군
  재현성 최고 수준. 공정 비교의 기준선
- 고전 baseline (GPSR/geographic, AODV/OLSR 계열)

## 문헌 작업 규칙

- Source of truth는 **라이브 Notion DB** `UAV/FANET RL 데일리 논문 분석`
  (`collection://82c87922-d396-48b4-b6b0-27d3f7217c90`). 2026-10-14 기준 85편
- census 파일은 조회 시점 스냅샷이다. "포화/공백"을 주장하기 전에 **반드시 DB를
  다시 조회**한다. 45편 스냅샷으로 판단했다가 틀린 전례가 있다
- DB의 Novelty·관련성·재현성 값은 작성자의 screening 판정이며 통계적 근거가 아니다
- "선행연구 0편" 주장은 제목 + `선택 이유` 전수 검색까지만 근거다. 투고 전 핵심
  논문은 본문·부록을 직접 확인한다

## 연구 무결성

- 코드에 구현된 것 / 실험으로 검증된 것 / 향후 계획을 구분한다
- 논문·코드·실험 결과에 없는 수치와 성능 우위를 만들지 않는다
- 불확실한 내용은 `needs-verification`으로 표시하고 확인 경로를 제시한다
- 구체적 성능 수치는 raw CSV와 manifest까지 추적 가능해야 한다
- smoke/partial/full 결과를 서로 대체하지 않는다
- 유리한 metric만 고르지 않고 실패 유형과 비용 지표를 함께 보고한다
- adapted baseline은 원 논문 충실도와 simulator adaptation 차이를 명시한다
- SwitchGLOBE 결과를 이 프로젝트 결과와 합치지 않는다

## 안전

- 이 저장소는 **public**이다. 미발표 원고, embargo 자료, 개인정보를 커밋하지 않는다
- 선행 저장소 `/home/alex/GLOBE_ROUTING`을 이 프로젝트 작업으로 수정하지 않는다
- `.env`, token, credential을 읽거나 출력하지 않는다
- 기존 census/artifact를 재조회 없이 덮어쓰지 않는다
- 파괴적 git 명령과 광범위 삭제를 실행하지 않는다
- GPU/Colab 장시간 실행과 외부 업로드는 사용자가 요청한 범위에서만 수행한다

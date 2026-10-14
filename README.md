# FANET Routing Research — Knowledge Base

이 저장소는 `GLOBE_ROUTING`(SwitchGLOBE 연구)에서 **알고리즘 구현과 독립적인
자산만** 추출한 것이다. SwitchGLOBE 자체(Global-to-Local distillation +
predictive risk switching)는 여기 없다 — 이 저장소는 **완전히 다른 접근법으로
처음부터 다시 시작하기 위한 참고 자료 모음**이다.

## 중요: 이 자료를 읽을 때 유의할 점

- `knowledge/`의 문서 대부분은 SwitchGLOBE라는 특정 방법론을 전제로 작성됐다.
  새 접근법에 그대로 적용되는 사실이 아니라 **배경지식·반면교사**로 취급한다.
- `knowledge/research_rigor_templates/`는 "현재 사실"이 아니라 "이전 프로젝트가
  엄격성을 어떻게 확보했는지"에 대한 템플릿이다. 새 프로젝트의 metric 정의,
  baseline 목록, seed/episode 수는 새로 정해야 한다.
- `skill-templates/`의 SKILL.md들은 `run_switchglobe.py`, `config/switchglobe.yaml`
  등 옛 저장소의 파일 경로를 참조한다. 새 프로젝트 구조에 맞게 경로와 명령을
  고쳐야 실제로 동작한다 — 그대로 복사해 쓰지 않는다.
- `references/`의 문헌 조사는 2026-09-05 기준 Notion
  `UAV/FANET RL 데일리 논문 분석` 데이터베이스 45편 + 추가 문헌을 반영한 것이다.
  최신 문헌은 다시 조사해야 한다.

## 구조

```text
references/              # 참고문헌 (bib, 45편 문헌 census, related-work 비교표)
knowledge/
  method_history.md             # Phase 1~13에서 통한 것/안 통한 것 (반면교사)
  old_method_summary.md         # 옛 Dec-POMDP 문제 정식화 + SwitchGLOBE 수식-코드 대응
  evaluation_metric_rationale.md# 문헌 기반 평가지표 선정 근거
  latency_optimization/         # 지연시간 최적화 조사 (후보 설계, 실험 프로토콜, 최종 리포트)
  research_rigor_templates/     # metric/baseline/experiment 계약, 주장-근거 매트릭스 템플릿
  legacy_analysis/              # 모델 선정 리포트, policy distillation causal ablation 분석
skill-templates/          # Claude Code SKILL.md 템플릿 (경로 수정 필요)
```

## 출처

원본: `GLOBE_ROUTING` 저장소 `refactor/globev2` 브랜치,
2026-10-14 추출. 원본 저장소의 구현 코드, checkpoint, artifact, 논문 LaTeX
초안은 포함하지 않았다.

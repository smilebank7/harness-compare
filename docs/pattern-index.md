# 패턴 색인

이 문서는 AI 코딩 하네스 설계에서 반복해서 등장하는 패턴을 찾는 색인입니다. 각 항목은 적용 챕터와 기존 하네스/근거 링크를 함께 제공합니다.

## 사용법

- 새 하네스를 설계할 때 먼저 패턴 이름을 고르고, 관련 챕터에서 tradeoff를 학습합니다.
- 그 다음 applicable harness/evidence link에서 실제 사례를 확인합니다.
- 패턴은 추천 순위가 아니라 설계 선택지입니다. 특정 패턴이 항상 우월하다는 의미가 아닙니다.

## 패턴 항목

| # | 패턴 | 적용 상황 | Applicable chapter | Applicable harness / evidence link | 오해 방지 메모 |
|---|------|-----------|--------------------|------------------------------------|----------------|
| 1 | 기본 도구·권한 substrate | 모델이 파일/명령/검색을 직접 다뤄야 할 때 | [tools](chapters/tools.md) | [opencode](../harnesses/opencode.md), [하네스 엔지니어링 비교](../comparisons/harness-engineering.md) | 도구가 많을수록 좋은 것이 아니라 권한 경계가 함께 필요하다. |
| 2 | Hashline anchor context | 모델이 소스 위치를 안정적으로 참조해야 할 때 | [context](chapters/context.md) | [omo](../harnesses/omo.md), [prompt 비교](../comparisons/prompt-engineering.md) | anchor는 근거 추적을 돕지만 stale 상태를 검증해야 한다. |
| 3 | Continuation loop | 긴 작업을 끊기지 않고 이어가야 할 때 | [loops](chapters/loops.md) | [omo](../harnesses/omo.md), [lazycodex](../harnesses/lazycodex.md) | continuation은 무한 반복을 막는 종료 조건과 함께 설계해야 한다. |
| 4 | Hook-driven guardrail | 도구 호출 전후에 정책과 상태를 강제해야 할 때 | [tools](chapters/tools.md), [loops](chapters/loops.md) | [omc](../harnesses/omc.md), [lazycodex](../harnesses/lazycodex.md) | hook이 많으면 행동이 불투명해질 수 있어 문서화가 필요하다. |
| 5 | Role-based orchestration | Planner/Architect/Critic/Executor처럼 책임을 분리해야 할 때 | [orchestration](chapters/orchestration.md) | [gajae-code](../harnesses/gajae-code.md), [omc](../harnesses/omc.md) | 역할 분리는 품질을 높이지만 조율 비용을 만든다. |
| 6 | Receipt ledger loop | 완료 주장과 검증 결과를 감사 가능하게 남겨야 할 때 | [loops](chapters/loops.md), [verification](chapters/verification.md) | [gajae-code](../harnesses/gajae-code.md), [루프 비교](../comparisons/loop-engineering.md) | receipt는 실제 검증을 대체하지 않고 검증 증거를 요약한다. |
| 7 | Spec-first evolution | 모호한 요구를 spec으로 고정하고 평가·진화 루프를 돌릴 때 | [loops](chapters/loops.md), [capstone](chapters/capstone.md) | [ouroboros](../harnesses/ouroboros.md), [루프 비교](../comparisons/loop-engineering.md) | spec을 빨리 쓰는 것보다 모호한 개념을 안정화하는 것이 먼저다. |
| 8 | Model routing table | 모델별 강점·비용·provider 차이를 명시적으로 다뤄야 할 때 | [model-routing](chapters/model-routing.md) | [gajae-code](../harnesses/gajae-code.md), [모델별 튜닝 비교](../comparisons/model-tuning.md) | fallback은 품질 저하를 숨기는 장치가 아니라 실패를 드러내는 경로여야 한다. |
| 9 | Prompt asset catalog | 모델별·역할별 prompt를 파일로 관리해야 할 때 | [prompts](chapters/prompts.md) | [opencode](../harnesses/opencode.md), [프롬프트 비교](../comparisons/prompt-engineering.md) | asset catalog는 중복을 줄이지만 runtime 조합 규칙이 필요하다. |
| 10 | Host-neutral adapter split | 같은 core를 여러 host/CLI에 붙여야 할 때 | [architecture](chapters/architecture.md) | [omo](../harnesses/omo.md), [아키텍처 비교](../comparisons/architecture.md) | adapter split은 porting을 돕지만 host 고유 기능이 희석될 수 있다. |
| 11 | Plugin distribution overlay | 기존 host 위에 기능 묶음을 배포해야 할 때 | [architecture](chapters/architecture.md), [tools](chapters/tools.md) | [lazycodex](../harnesses/lazycodex.md), [omo](../harnesses/omo.md) | 배포 편의성과 runtime 독립성은 별개의 tradeoff다. |
| 12 | Team verify/fix loop | 여러 agent가 구현·검증·수정을 반복해야 할 때 | [orchestration](chapters/orchestration.md), [verification](chapters/verification.md) | [omc](../harnesses/omc.md), [루프 비교](../comparisons/loop-engineering.md) | 병렬화는 작업 분할이 명확할 때만 이득이다. |
| 13 | Evidence-linked comparison | 설계 비교가 출처 없는 의견으로 흐르지 않게 해야 할 때 | [verification](chapters/verification.md), [mental-model](chapters/mental-model.md) | [framework](../framework.md), [README 검증 기준](../README.md#검증-기준) | 근거 링크는 장식이 아니라 주장의 검증 가능성을 만든다. |
| 14 | Capstone design chain | 학습 결과를 분석→설계→구현 계획으로 검증해야 할 때 | [capstone](chapters/capstone.md) | [rubric](exercises/rubric.md), [framework](../framework.md) | capstone은 큰 구현을 강제하지 않고 설계 판단을 검증한다. |

## 패턴별 빠른 경로

### 하네스 표면을 설계할 때

1. [기본 도구·권한 substrate](#패턴-항목)
2. [Hook-driven guardrail](#패턴-항목)
3. [Host-neutral adapter split](#패턴-항목)

### 루프와 완료 검증을 설계할 때

1. [Continuation loop](#패턴-항목)
2. [Receipt ledger loop](#패턴-항목)
3. [Spec-first evolution](#패턴-항목)
4. [Team verify/fix loop](#패턴-항목)

### 모델·프롬프트를 설계할 때

1. [Prompt asset catalog](#패턴-항목)
2. [Model routing table](#패턴-항목)
3. [Role-based orchestration](#패턴-항목)

## Backlinks

- [학습 경로](learning-path.md)
- [문서 맵](document-map.md)
- [용어집](glossary.md)
- [개념 색인](concept-index.md)
- [framework](../framework.md)

# 학습 경로

`harness-compare`를 처음 읽는 독자는 이 문서를 선형 과정으로 사용합니다. 이미 특정 개념을 찾고 있다면 [문서 맵](document-map.md), [용어집](glossary.md), [개념 색인](concept-index.md), [패턴 색인](pattern-index.md)으로 이동해도 됩니다.

## 목표

최종 목표는 여섯 개 AI 코딩 하네스 분석을 단순히 암기하는 것이 아니라, **새 AI 코딩 하네스를 직접 설계할 수 있는 판단력**을 얻는 것입니다. 완주자는 다음 세 가지 산출물을 만들 수 있어야 합니다.

1. 기존 하네스를 5개 비교 차원으로 분석한 보고서
2. 근거 링크가 붙은 새 하네스 설계안
3. toy harness/loop 구현 계획

## 대상 독자

- AI 코딩 에이전트, 하네스, 워크플로 설계를 배우고 싶은 독자
- `opencode`, `Claude Code`, `Codex`, `Ouroboros`류 도구의 설계 차이를 구조적으로 비교하고 싶은 독자
- 이미 문서를 많이 읽었지만 “그래서 내가 설계한다면 무엇을 결정해야 하는가?”가 흐릿한 독자

## 5단계 과정

| 단계 | 이름 | 핵심 질문 | 읽을 문서 | 완료 체크 |
|------|------|-----------|-----------|-----------|
| 1 | 지도 읽기 | 이 레포의 분석 기준과 문서 지형은 무엇인가? | [README](../README.md), [문서 맵](document-map.md), [map 챕터](chapters/map.md), [framework](../framework.md) | 5개 비교 차원과 `docs/` 학습 레이어의 위치를 설명할 수 있다. |
| 2 | 개념 학습 | 하네스 설계에서 반복해서 나오는 핵심 용어는 무엇인가? | [용어집](glossary.md), [개념 색인](concept-index.md), [mental-model](chapters/mental-model.md) | 하네스/루프/프롬프트/아키텍처/모델 라우팅의 차이를 말할 수 있다. |
| 3 | 하네스 분석 | 여섯 하네스는 같은 문제를 어떻게 다르게 풀었는가? | [opencode](../harnesses/opencode.md), [omo](../harnesses/omo.md), [lazycodex](../harnesses/lazycodex.md), [omc](../harnesses/omc.md), [gajae-code](../harnesses/gajae-code.md), [ouroboros](../harnesses/ouroboros.md) | 각 하네스의 설계 접근을 가치중립 명사구로 요약할 수 있다. |
| 4 | 패턴 종합 | 반복되는 설계 패턴과 tradeoff는 무엇인가? | [패턴 색인](pattern-index.md), [context](chapters/context.md), [tools](chapters/tools.md), [prompts](chapters/prompts.md), [loops](chapters/loops.md), [orchestration](chapters/orchestration.md), [architecture](chapters/architecture.md), [model-routing](chapters/model-routing.md), [verification](chapters/verification.md) | 새 하네스 설계에서 선택해야 할 decision driver를 5개 이상 뽑을 수 있다. |
| 5 | capstone | 내 설계는 분석·근거·검증을 모두 갖췄는가? | [capstone](chapters/capstone.md), [분석 보고서 템플릿](exercises/analysis-report-template.md), [설계안 템플릿](exercises/design-proposal-template.md), [toy 구현 계획 템플릿](exercises/toy-implementation-plan-template.md), [루브릭](exercises/rubric.md), [예시](exercises/examples.md) | 분석 보고서 → 설계안 → toy 구현 계획 3단계 산출물을 완성한다. |

## 단계별 읽기 전략

### 1단계 — 지도 읽기

먼저 [README](../README.md)의 분석 대상 표와 요약 매트릭스를 읽습니다. 여기서 목적은 어떤 하네스가 “좋은지” 고르는 것이 아니라, 각 하네스가 어떤 설계 표면을 노출하는지 보는 것입니다. 이어서 [framework](../framework.md)의 5개 비교 차원 정의를 읽고, 이 레포가 어떤 기준으로 주장과 인용을 관리하는지 확인합니다.

완료 질문:

- 하네스 엔지니어링과 루프 엔지니어링의 경계는 어디인가?
- 왜 README 매트릭스는 가치중립 명사구만 쓰는가?
- `docs/` 학습 레이어와 기존 분석 레이어는 어떻게 나뉘는가?

### 2단계 — 개념 학습

[용어집](glossary.md)은 단어의 짧은 정의를 제공합니다. [개념 색인](concept-index.md)은 개념을 “어디서 배우고 어디서 근거를 확인할지”로 안내합니다. 모르는 단어가 나오면 용어집에서 정의를 보고, 해당 개념의 primary doc과 evidence doc으로 이동합니다.

완료 질문:

- 도구 표면, 권한 경계, 컨텍스트 공급은 왜 같은 차원 안에서도 다른 결정인가?
- 프롬프트는 단순 system prompt가 아니라 어떤 역할 계약을 포함하는가?
- 모델 라우팅과 fallback은 설계 품질을 어떻게 바꾸는가?

### 3단계 — 하네스 분석

여섯 하네스 문서는 동일한 공통 골격을 따릅니다. 각 문서에서 먼저 머리의 기준 SHA와 분석일을 확인하고, 5개 차원 섹션을 읽습니다. 모든 세부 구현을 외우지 말고 “이 하네스가 무엇을 substrate로 삼고, 어떤 루프를 돌리고, 어떤 prompt contract를 주입하고, 어떤 런타임 경계를 갖는가”를 정리합니다.

완료 질문:

- opencode가 기준선인 이유는 무엇인가?
- 플러그인형 하네스와 독립형 하네스의 상태 경계는 어떻게 다른가?
- Ouroboros의 스펙 주도 루프는 일반 coding harness와 무엇이 다른가?

### 4단계 — 패턴 종합

[패턴 색인](pattern-index.md)에서 반복되는 설계 패턴을 고릅니다. 그런 다음 관련 챕터와 기존 분석 문서를 왕복합니다. 예를 들어 “receipt ledger loop”를 보면 [loops](chapters/loops.md)에서 개념을 배우고, [gajae-code 분석](../harnesses/gajae-code.md)과 [loop-engineering 비교](../comparisons/loop-engineering.md)에서 근거를 확인합니다.

완료 질문:

- 같은 “검증”이라도 훅, ledger, evaluator, critic은 각각 무엇을 책임지는가?
- 서브에이전트 오케스트레이션은 언제 품질을 올리고 언제 비용만 늘리는가?
- 모델별 튜닝은 prompt asset, routing table, adapter 중 어디에 위치하는가?

### 5단계 — capstone

마지막에는 [capstone](chapters/capstone.md)의 안내에 따라 세 산출물을 작성합니다. 첫째, 기존 또는 가상의 하네스를 분석 보고서로 분해합니다. 둘째, 새 하네스 설계안을 작성합니다. 셋째, toy 구현 계획으로 scope를 줄여 실제 구현 가능한 단위까지 내립니다. [루브릭](exercises/rubric.md)은 “그럴듯한 글”과 “근거 있는 설계”를 구분하는 기준입니다.

완료 질문:

- 내 설계안의 각 결정은 어떤 기존 문서 근거를 갖는가?
- 검증 루프와 종료 조건은 실제로 테스트 가능한가?
- toy 구현 계획은 너무 크거나 너무 추상적이지 않은가?

## 단계별 완료 체크리스트

- [ ] 5개 비교 차원의 정의와 경계를 설명했다.
- [ ] 핵심 용어 30개 중 모르는 항목을 용어집에서 확인했다.
- [ ] 여섯 하네스 분석 문서의 5개 차원 섹션을 모두 훑었다.
- [ ] 패턴 색인에서 최소 10개 패턴을 관련 챕터와 evidence doc으로 왕복했다.
- [ ] capstone 3단계 산출물을 루브릭으로 자기 평가했다.

## 관련 링크

- [문서 맵](document-map.md)
- [용어집](glossary.md)
- [개념 색인](concept-index.md)
- [패턴 색인](pattern-index.md)
- [capstone 챕터](chapters/capstone.md)
- [루브릭](exercises/rubric.md)

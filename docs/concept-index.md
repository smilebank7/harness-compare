# 개념 색인

이 문서는 개념에서 출발해 학습 챕터와 기존 근거 문서로 이동하는 위키형 색인입니다. 짧은 정의는 [용어집](glossary.md)을 보고, 학습 순서는 [학습 경로](learning-path.md)를 따릅니다.

## 사용법

- **Primary doc**은 개념을 학습용으로 설명하는 첫 번째 문서입니다.
- **Evidence doc**은 기존 분석 레이어에서 근거를 확인하는 문서입니다.
- 설계안을 작성할 때는 각 개념의 evidence doc을 최소 하나 이상 인용합니다.

## 개념 항목

| # | 개념 | Primary doc | Evidence doc | 함께 볼 문서 |
|---|------|-------------|--------------|--------------|
| 1 | 5개 비교 차원 | [map](chapters/map.md) | [framework](../framework.md) | [README 요약 매트릭스](../README.md#요약-매트릭스) |
| 2 | 제작자 관점 비교 | [mental-model](chapters/mental-model.md) | [framework](../framework.md) | [하네스 엔지니어링 비교](../comparisons/harness-engineering.md) |
| 3 | 가치중립 설계 언어 | [mental-model](chapters/mental-model.md) | [README 요약 매트릭스](../README.md#요약-매트릭스) | [framework](../framework.md) |
| 4 | 기준선과 파생 비교 | [map](chapters/map.md) | [opencode 분석](../harnesses/opencode.md) | [omo 분석](../harnesses/omo.md), [lazycodex 분석](../harnesses/lazycodex.md) |
| 5 | 도구 표면 설계 | [tools](chapters/tools.md) | [하네스 엔지니어링 비교](../comparisons/harness-engineering.md) | [opencode 분석](../harnesses/opencode.md) |
| 6 | 권한 경계 | [tools](chapters/tools.md) | [framework](../framework.md) | [gajae-code 분석](../harnesses/gajae-code.md) |
| 7 | 컨텍스트 공급 | [context](chapters/context.md) | [하네스 엔지니어링 비교](../comparisons/harness-engineering.md) | [ouroboros 분석](../harnesses/ouroboros.md) |
| 8 | 컨텍스트 압축과 선별 | [context](chapters/context.md) | [gajae-code 분석](../harnesses/gajae-code.md) | [ouroboros 분석](../harnesses/ouroboros.md) |
| 9 | 프롬프트 asset | [prompts](chapters/prompts.md) | [프롬프트 엔지니어링 비교](../comparisons/prompt-engineering.md) | [opencode 분석](../harnesses/opencode.md) |
| 10 | 역할 지시와 persona | [prompts](chapters/prompts.md) | [gajae-code 분석](../harnesses/gajae-code.md) | [omc 분석](../harnesses/omc.md) |
| 11 | hook 기반 확장 | [tools](chapters/tools.md) | [omc 분석](../harnesses/omc.md) | [lazycodex 분석](../harnesses/lazycodex.md) |
| 12 | ledger/receipt 상태 관리 | [loops](chapters/loops.md) | [gajae-code 분석](../harnesses/gajae-code.md) | [verification](chapters/verification.md) |
| 13 | 검증 루프 | [verification](chapters/verification.md) | [루프 엔지니어링 비교](../comparisons/loop-engineering.md) | [gajae-code 분석](../harnesses/gajae-code.md) |
| 14 | continuation loop | [loops](chapters/loops.md) | [omo 분석](../harnesses/omo.md) | [lazycodex 분석](../harnesses/lazycodex.md) |
| 15 | multi-agent team loop | [orchestration](chapters/orchestration.md) | [omc 분석](../harnesses/omc.md) | [루프 엔지니어링 비교](../comparisons/loop-engineering.md) |
| 16 | Planner/Architect/Critic 합의 | [orchestration](chapters/orchestration.md) | [gajae-code 분석](../harnesses/gajae-code.md) | [verification](chapters/verification.md) |
| 17 | 스펙 주도 루프 | [loops](chapters/loops.md) | [ouroboros 분석](../harnesses/ouroboros.md) | [capstone](chapters/capstone.md) |
| 18 | ambiguity scoring | [verification](chapters/verification.md) | [ouroboros 분석](../harnesses/ouroboros.md) | [capstone](chapters/capstone.md) |
| 19 | 플러그인형 아키텍처 | [architecture](chapters/architecture.md) | [omo 분석](../harnesses/omo.md) | [lazycodex 분석](../harnesses/lazycodex.md) |
| 20 | 독립형 CLI/runtime 아키텍처 | [architecture](chapters/architecture.md) | [gajae-code 분석](../harnesses/gajae-code.md) | [아키텍처 비교](../comparisons/architecture.md) |
| 21 | model routing | [model-routing](chapters/model-routing.md) | [모델별 튜닝 비교](../comparisons/model-tuning.md) | [gajae-code 분석](../harnesses/gajae-code.md) |
| 22 | fallback chain | [model-routing](chapters/model-routing.md) | [omo 분석](../harnesses/omo.md) | [모델별 튜닝 비교](../comparisons/model-tuning.md) |
| 23 | citation discipline | [verification](chapters/verification.md) | [framework](../framework.md) | [README 검증 기준](../README.md#검증-기준) |
| 24 | capstone evidence chain | [capstone](chapters/capstone.md) | [framework](../framework.md) | [분석 보고서 템플릿](exercises/analysis-report-template.md), [루브릭](exercises/rubric.md) |
| 25 | toy implementation scope | [capstone](chapters/capstone.md) | [gajae-code 분석](../harnesses/gajae-code.md) | [toy 구현 계획 템플릿](exercises/toy-implementation-plan-template.md) |
| 26 | 문서화된 완료 주장 | [verification](chapters/verification.md) | [README 검증 기준](../README.md#검증-기준) | [루브릭](exercises/rubric.md) |

## 개념 클러스터

### 읽기 전 개념

- [5개 비교 차원](#개념-항목)
- [제작자 관점 비교](#개념-항목)
- [가치중립 설계 언어](#개념-항목)
- [기준선과 파생 비교](#개념-항목)

### 설계 표면 개념

- 도구 표면 설계
- 권한 경계
- 컨텍스트 공급
- 프롬프트 asset
- 역할 지시와 persona

### 실행 루프 개념

- ledger/receipt 상태 관리
- 검증 루프
- continuation loop
- multi-agent team loop
- 스펙 주도 루프

### 출판·검증 개념

- citation discipline
- 문서화된 완료 주장
- capstone evidence chain
- toy implementation scope

## Backlinks

- [학습 경로](learning-path.md)
- [문서 맵](document-map.md)
- [용어집](glossary.md)
- [패턴 색인](pattern-index.md)
- [framework](../framework.md)

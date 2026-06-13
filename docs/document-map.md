# 문서 맵

이 문서는 `harness-compare` 학습 교과서·위키 레이어의 지도를 고정합니다. 기존 분석 문서(`framework.md`, `harnesses/*.md`, `comparisons/*.md`)는 근거 레이어이고, `docs/`는 학습 경로·색인·챕터·실습을 제공하는 별도 학습 레이어입니다.

## 범위와 제약

- v1 학습 레이어는 Markdown + Mermaid/ASCII만 사용합니다.
- 정적 사이트 설정(MkDocs/Docusaurus), 벤치마크, 추천·랭킹·별점, 영어 번역, 자동 갱신 파이프라인은 범위 밖입니다.
- 코드 식별자, 파일 경로, 명령어, 원문 인용은 원어를 유지할 수 있지만 학습 설명은 한국어로 작성합니다.

## 문서 구조

```text
harness-compare/
├─ README.md
├─ framework.md
├─ harnesses/
├─ comparisons/
└─ docs/
   ├─ learning-path.md
   ├─ glossary.md
   ├─ concept-index.md
   ├─ document-map.md
   ├─ pattern-index.md
   ├─ chapters/
   │  ├─ map.md
   │  ├─ mental-model.md
   │  ├─ context.md
   │  ├─ tools.md
   │  ├─ prompts.md
   │  ├─ loops.md
   │  ├─ orchestration.md
   │  ├─ architecture.md
   │  ├─ model-routing.md
   │  ├─ verification.md
   │  └─ capstone.md
   └─ exercises/
      ├─ analysis-report-template.md
      ├─ design-proposal-template.md
      ├─ toy-implementation-plan-template.md
      ├─ examples.md
      └─ rubric.md
```

## Expected docs 목록

아래 목록은 실행 acceptance와 verification의 기준입니다. 표의 파일이 누락되면 실패입니다. 표에 없는 `docs/` 파일을 추가하면 이 문서에 등재하고 관련 링크/backlinks를 추가해야 합니다.

| Kind | Path | 용도 | 필수 링크 |
|------|------|------|-----------|
| Gateway | [`../README.md`](../README.md) | 프로젝트 소개, 분석 대상, 요약 매트릭스, 학습 교과서·위키 진입점 | learning path, 색인, 챕터, capstone |
| Framework | [`../framework.md`](../framework.md) | 5개 비교 차원, 인용 규칙, 동결된 판단 기준 | 각 색인과 챕터의 근거 링크 |
| Learning path | [`learning-path.md`](learning-path.md) | 지도 읽기 → 개념 학습 → 하네스 분석 → 패턴 종합 → capstone 5단계 과정 | document map, glossary, concept index, pattern index, capstone |
| Glossary | [`glossary.md`](glossary.md) | 핵심 용어 30개 이상 | 각 항목: 관련 챕터 ≥1, 기존 분석 또는 framework 링크 ≥1 |
| Concept index | [`concept-index.md`](concept-index.md) | 개념별 탐색 색인 20개 이상 | 각 항목: primary doc ≥1, evidence doc ≥1 |
| Document map | [`document-map.md`](document-map.md) | 전체 문서 지형과 backlinks 규칙 | README, learning path, 모든 docs 문서 |
| Pattern index | [`pattern-index.md`](pattern-index.md) | 설계 패턴 색인 10개 이상 | 각 항목: applicable chapter ≥1, applicable harness/evidence link ≥1 |
| Chapter | [`chapters/map.md`](chapters/map.md) | 전체 문서 지형과 학습 방법 | evidence links, related links, backlinks |
| Chapter | [`chapters/mental-model.md`](chapters/mental-model.md) | 제작자 관점과 비교 사고법 | evidence links, related links, backlinks |
| Chapter | [`chapters/context.md`](chapters/context.md) | 컨텍스트 공급 설계 | evidence links, related links, backlinks |
| Chapter | [`chapters/tools.md`](chapters/tools.md) | 도구 표면과 권한 경계 | evidence links, related links, backlinks |
| Chapter | [`chapters/prompts.md`](chapters/prompts.md) | 프롬프트와 역할 지시 설계 | evidence links, related links, backlinks |
| Chapter | [`chapters/loops.md`](chapters/loops.md) | 반복 제어와 검증 루프 | evidence links, related links, backlinks |
| Chapter | [`chapters/orchestration.md`](chapters/orchestration.md) | 서브에이전트와 팀 조율 | evidence links, related links, backlinks |
| Chapter | [`chapters/architecture.md`](chapters/architecture.md) | 배포 형태와 상태 경계 | evidence links, related links, backlinks |
| Chapter | [`chapters/model-routing.md`](chapters/model-routing.md) | 모델 분기와 fallback | evidence links, related links, backlinks |
| Chapter | [`chapters/verification.md`](chapters/verification.md) | 완료 주장 검증 | evidence links, related links, backlinks |
| Chapter | [`chapters/capstone.md`](chapters/capstone.md) | 최종 종합 과제 안내 | exercises, rubric, backlinks |
| Exercise | [`exercises/analysis-report-template.md`](exercises/analysis-report-template.md) | 분석 보고서 템플릿 | capstone, rubric, evidence docs |
| Exercise | [`exercises/design-proposal-template.md`](exercises/design-proposal-template.md) | 설계안 템플릿 | capstone, rubric, evidence docs |
| Exercise | [`exercises/toy-implementation-plan-template.md`](exercises/toy-implementation-plan-template.md) | toy 구현 계획 템플릿 | capstone, rubric, evidence docs |
| Exercise | [`exercises/examples.md`](exercises/examples.md) | 짧은 완성 예시 | capstone, templates, rubric |
| Exercise | [`exercises/rubric.md`](exercises/rubric.md) | capstone 평가 기준 | capstone, templates, examples |

## Backlinks 규칙

1. 모든 `docs/` 문서는 끝부분에 `## 관련 링크` 또는 `## Backlinks` 섹션을 둡니다.
2. 챕터 문서는 최소한 이 문서, `learning-path.md`, 하나 이상의 색인 문서, 하나 이상의 기존 근거 문서(`framework.md`, `harnesses/*.md`, `comparisons/*.md`)로 연결합니다.
3. 색인 항목은 항목 단위로 required link를 충족해야 합니다. 문서 상단의 일반 링크만으로 항목별 링크 품질을 대체하지 않습니다.
4. 다이어그램이 있는 챕터는 다이어그램 바로 앞뒤 같은 소제목 범위에 고정 마커 `캡션:`과 `텍스트 설명:`을 둡니다.
5. 새 문서가 기존 분석의 판단을 재서술할 때는 가능한 한 `framework.md`, `harnesses/*.md`, `comparisons/*.md` 링크를 근거로 붙입니다.

## 문서별 작성 순서

1. 이 문서로 expected file list와 backlinks 규칙을 고정합니다.
2. [`learning-path.md`](learning-path.md)와 세 색인([`glossary.md`](glossary.md), [`concept-index.md`](concept-index.md), [`pattern-index.md`](pattern-index.md))을 작성합니다.
3. [`chapters/map.md`](chapters/map.md), [`chapters/mental-model.md`](chapters/mental-model.md)를 먼저 작성해 독자의 관점과 지도 읽기를 정렬합니다.
4. context/tools/prompts/loops/orchestration/architecture/model-routing/verification 챕터를 작성합니다.
5. [`chapters/capstone.md`](chapters/capstone.md)와 [`exercises/`](exercises/rubric.md)를 연결합니다.
6. 마지막에 [`../README.md`](../README.md)에 학습 관문 섹션을 추가하되 기존 분석 매트릭스와 문서 목록은 보존합니다.

## 검수 체크

- [ ] Expected docs 목록의 모든 `docs/` 파일이 존재한다.
- [ ] `README.md`가 기존 분석 대상 표와 6×5 요약 매트릭스를 유지한다.
- [ ] `learning-path.md`가 5단계를 정확한 순서로 제공한다.
- [ ] glossary 항목이 30개 이상이고 항목별 관련 챕터와 기존 근거 링크가 있다.
- [ ] concept-index 항목이 20개 이상이고 항목별 primary doc과 evidence doc이 있다.
- [ ] pattern-index 항목이 10개 이상이고 항목별 applicable chapter와 evidence link가 있다.
- [ ] 11개 챕터가 모두 필수 섹션과 다이어그램 marker(`캡션:`, `텍스트 설명:`)를 포함한다.
- [ ] exercises 5개 파일이 templates, example, checklist, rubric 요구를 충족한다.
- [ ] 추천·랭킹·별점·벤치마크·영어 번역·정적 사이트 설정이 추가되지 않았다.

## 관련 링크

- [학습 경로](learning-path.md)
- [용어집](glossary.md)
- [개념 색인](concept-index.md)
- [패턴 색인](pattern-index.md)
- [프레임워크](../framework.md)
- [README 관문](../README.md)

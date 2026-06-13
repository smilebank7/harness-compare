# harness-compare

AI 코딩 하네스 6종의 설계 결정과 그 근거를 **제작자 관점**에서 해부하는 한국어 비교 분석 레포입니다. "어떤 하네스를 골라 쓸까"가 아니라 "하네스를 어떻게 설계했고 왜 그랬나"를 다룹니다. 모든 설계 주장은 분석 시점 커밋에 고정된 소스 permalink로 뒷받침됩니다.

## 분석 대상

| 약칭 | 레포 | 성격 |
|------|------|------|
| opencode | [sst/opencode](https://github.com/sst/opencode) | 베이스 플랫폼 (기준선) |
| omo | [code-yeongyu/oh-my-openagent](https://github.com/code-yeongyu/oh-my-openagent) | OpenCode 플러그인 하네스 (구 oh-my-opencode) |
| lazy-codex | [code-yeongyu/lazycodex](https://github.com/code-yeongyu/lazycodex) | OmO의 Codex 배포판 |
| omc | [Yeachan-Heo/oh-my-claudecode](https://github.com/Yeachan-Heo/oh-my-claudecode) | Claude Code 멀티에이전트 오케스트레이션 |
| gajae-code | [Yeachan-Heo/gajae-code](https://github.com/Yeachan-Heo/gajae-code) | 독립형 코딩 에이전트 하네스 |
| ooo | [Q00/ouroboros](https://github.com/Q00/ouroboros) | 스펙 주도 자율 개발 루프 |

## 요약 매트릭스

각 셀은 가치중립 설계 접근 명사구와 해당 심층 문서 앵커를 함께 둡니다. 차원 정의는 [framework.md](framework.md#5개-비교-차원의-조작적-정의)를 단일 기준으로 따릅니다.

| 하네스 | 하네스 엔지니어링 | 루프 엔지니어링 | 프롬프트 엔지니어링 | 아키텍처 | 모델별 튜닝 |
|--------|------------------|----------------|----------------------|------------|---------------|
| opencode | [기본 도구·권한 substrate](harnesses/opencode.md#하네스-엔지니어링) | [3치 스텝 루프](harnesses/opencode.md#루프-엔지니어링) | [모델별 `.txt` prompt asset](harnesses/opencode.md#프롬프트-엔지니어링) | [SDK-bound plugin host](harnesses/opencode.md#아키텍처) | [모델 ID 기반 prompt/tool transform](harnesses/opencode.md#모델별-튜닝) |
| omo | [플러그인 도구 증폭·hashline anchor](harnesses/omo.md#하네스-엔지니어링) | [todo·ralph continuation loop](harnesses/omo.md#루프-엔지니어링) | [runtime 합성 agent persona](harnesses/omo.md#프롬프트-엔지니어링) | [host-neutral core + adapter split](harnesses/omo.md#아키텍처) | [category·agent fallback routing](harnesses/omo.md#모델별-튜닝) |
| lazy-codex | [external process hook bundle](harnesses/lazycodex.md#하네스-엔지니어링) | [Stop hook + `.omo` ledger loop](harnesses/lazycodex.md#루프-엔지니어링) | [Codex compatibility overlay skills](harnesses/lazycodex.md#프롬프트-엔지니어링) | [thin distribution + marketplace bundle](harnesses/lazycodex.md#아키텍처) | [OpenAI role TOML/profile catalog](harnesses/lazycodex.md#모델별-튜닝) |
| omc | [Claude Code all-event hook mesh](harnesses/omc.md#하네스-엔지니어링) | [Team verify/fix state loop](harnesses/omc.md#루프-엔지니어링) | [skill·role Markdown injection](harnesses/omc.md#프롬프트-엔지니어링) | [plugin+CLI+bridge package](harnesses/omc.md#아키텍처) | [prompt signal tier router](harnesses/omc.md#모델별-튜닝) |
| gajae-code | [external runner ToolSession surface](harnesses/gajae-code.md#하네스-엔지니어링) | [workflow·receipt ledger loop](harnesses/gajae-code.md#루프-엔지니어링) | [source-bundled workflow contracts](harnesses/gajae-code.md#프롬프트-엔지니어링) | [`gjc` CLI + `.gjc` runtime boundary](harnesses/gajae-code.md#아키텍처) | [selector/profile/thinking suffix binding](harnesses/gajae-code.md#모델별-튜닝) |
| ooo | [Seed/Workflow IR context substrate](harnesses/ouroboros.md#하네스-엔지니어링) | [ambiguity·evaluation·evolution gates](harnesses/ouroboros.md#루프-엔지니어링) | [Socratic·Seed·Evaluator persona chain](harnesses/ouroboros.md#프롬프트-엔지니어링) | [Python workflow OS + plugin firewall](harnesses/ouroboros.md#아키텍처) | [PAL complexity tier adapter routing](harnesses/ouroboros.md#모델별-튜닝) |

## 학습 교과서·위키로 읽기

이 레포는 기존 분석 문서를 보존하면서, `docs/` 아래에 한국어 학습 교과서·위키 레이어를 추가했습니다. 처음 읽는 독자는 [학습 경로](docs/learning-path.md)를 따라가고, 특정 개념을 찾는 독자는 [문서 맵](docs/document-map.md), [용어집](docs/glossary.md), [개념 색인](docs/concept-index.md), [패턴 색인](docs/pattern-index.md)을 사용하면 됩니다.

- 선형 과정: [지도 읽기 → 개념 학습 → 하네스 분석 → 패턴 종합 → capstone](docs/learning-path.md)
- 위키 탐색: [문서 맵](docs/document-map.md), [용어집](docs/glossary.md), [개념 색인](docs/concept-index.md), [패턴 색인](docs/pattern-index.md)
- 교과서 챕터: [map](docs/chapters/map.md), [mental-model](docs/chapters/mental-model.md), [context](docs/chapters/context.md), [tools](docs/chapters/tools.md), [prompts](docs/chapters/prompts.md), [loops](docs/chapters/loops.md), [orchestration](docs/chapters/orchestration.md), [architecture](docs/chapters/architecture.md), [model-routing](docs/chapters/model-routing.md), [verification](docs/chapters/verification.md), [capstone](docs/chapters/capstone.md)
- Capstone 실습: [분석 보고서 템플릿](docs/exercises/analysis-report-template.md), [설계안 템플릿](docs/exercises/design-proposal-template.md), [toy 구현 계획 템플릿](docs/exercises/toy-implementation-plan-template.md), [예시](docs/exercises/examples.md), [루브릭](docs/exercises/rubric.md)

## 문서 목록

### 하네스별 심층 분석

- [opencode — 기준선 심층 분석](harnesses/opencode.md)
- [omo (oh-my-openagent) — 심층 분석](harnesses/omo.md)
- [lazycodex (LazyCodex) — 심층 분석](harnesses/lazycodex.md)
- [oh-my-claudecode (OMC) — 심층 분석](harnesses/omc.md)
- [gajae-code (Gajae-Code / GJC) — 심층 분석](harnesses/gajae-code.md)
- [Ouroboros (ooo) — 심층 분석](harnesses/ouroboros.md)

### 차원별 횡단 비교

- [하네스 엔지니어링 횡단 비교](comparisons/harness-engineering.md)
- [루프 엔지니어링 횡단 비교](comparisons/loop-engineering.md)
- [프롬프트 엔지니어링 횡단 비교](comparisons/prompt-engineering.md)
- [아키텍처 횡단 비교](comparisons/architecture.md)
- [모델별 튜닝 횡단 비교](comparisons/model-tuning.md)

### 학습 교과서·위키

- [학습 경로](docs/learning-path.md)
- [문서 맵](docs/document-map.md)
- [용어집](docs/glossary.md)
- [개념 색인](docs/concept-index.md)
- [패턴 색인](docs/pattern-index.md)
- [교과서 챕터: map](docs/chapters/map.md)
- [교과서 챕터: mental-model](docs/chapters/mental-model.md)
- [교과서 챕터: context](docs/chapters/context.md)
- [교과서 챕터: tools](docs/chapters/tools.md)
- [교과서 챕터: prompts](docs/chapters/prompts.md)
- [교과서 챕터: loops](docs/chapters/loops.md)
- [교과서 챕터: orchestration](docs/chapters/orchestration.md)
- [교과서 챕터: architecture](docs/chapters/architecture.md)
- [교과서 챕터: model-routing](docs/chapters/model-routing.md)
- [교과서 챕터: verification](docs/chapters/verification.md)
- [교과서 챕터: capstone](docs/chapters/capstone.md)
- [Capstone 분석 보고서 템플릿](docs/exercises/analysis-report-template.md)
- [Capstone 설계안 템플릿](docs/exercises/design-proposal-template.md)
- [Capstone toy 구현 계획 템플릿](docs/exercises/toy-implementation-plan-template.md)
- [Capstone 예시](docs/exercises/examples.md)
- [Capstone 루브릭](docs/exercises/rubric.md)


## 디렉토리 안내

```
harness-compare/
├─ README.md       # 요약 매트릭스 + 학습 교과서·위키 관문
├─ framework.md    # 5개 비교 차원의 조작적 정의와 인용 규칙
├─ harnesses/      # 하네스별 심층 분석 6편
├─ comparisons/    # 차원별 횡단 비교 에세이 5편
└─ docs/           # 학습 경로, 색인, 교과서 챕터, capstone 실습
```

## 검증 기준

- 분석 기준 커밋은 [framework.md 부록](framework.md#부록-분석-기준-커밋)에 고정했습니다.
- 심층 분석과 횡단 비교의 GitHub 소스 링크는 고정 SHA와 line range를 포함합니다.
- 5개 비교 차원은 [framework.md v1.0 (C2 동결)](framework.md#개정-이력) 정의를 따릅니다.
- README 매트릭스 셀은 가치중립 설계 접근 명사구와 심층 문서 앵커만 담습니다.

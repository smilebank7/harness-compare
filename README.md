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

> 작성 중 — 심층 분석 문서가 완성되는 대로 채워집니다.

## 디렉토리 안내

```
harness-compare/
├─ README.md       # 요약 매트릭스 + 내비게이션 (현재 문서)
├─ framework.md    # 5개 비교 차원의 조작적 정의 (작성 예정)
├─ harnesses/      # 하네스별 심층 분석 6편
└─ comparisons/    # 차원별 횡단 비교 에세이 5편
```

## 비교 차원

하네스 엔지니어링 · 루프 엔지니어링 · 프롬프트 엔지니어링 · 아키텍처 · 모델별 튜닝 — 각 차원의 조작적 정의는 `framework.md`를 단일 기준으로 따릅니다.

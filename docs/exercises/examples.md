# Capstone 짧은 완성 예시

## 목적

이 문서는 capstone 산출물이 어느 정도 구체적이어야 하는지 보여주는 짧은 예시입니다. 완성 제품 설계가 아니라, 분석 → 설계 → toy 구현 계획이 어떻게 이어지는지 보여줍니다.

## 짧은 완성 예시

### 1. 분석 보고서 요약

- 대상: `gajae-code`
- 범위: workflow·receipt ledger loop 중심
- 관련 근거:
  - [gajae-code 분석](../../harnesses/gajae-code.md)
  - [루프 엔지니어링 비교](../../comparisons/loop-engineering.md)
  - [verification 챕터](../chapters/verification.md)

| 차원 | 가치중립 설계 접근 명사구 | 근거 | tradeoff |
|------|---------------------------|------|----------|
| 하네스 엔지니어링 | external runner ToolSession surface | [gajae-code 분석](../../harnesses/gajae-code.md) | 도구 표면이 풍부하지만 권한 규칙이 중요하다. |
| 루프 엔지니어링 | workflow·receipt ledger loop | [루프 비교](../../comparisons/loop-engineering.md) | 감사 가능성이 높지만 checkpoint 설계가 무거워진다. |
| 프롬프트 엔지니어링 | source-bundled workflow contracts | [프롬프트 비교](../../comparisons/prompt-engineering.md) | 역할 계약이 명확하지만 prompt 유지보수가 필요하다. |
| 아키텍처 | CLI + repo-local runtime boundary | [아키텍처 비교](../../comparisons/architecture.md) | repo-local audit trail이 생기지만 ignore/commit 정책이 필요하다. |
| 모델별 튜닝 | selector/profile binding | [모델별 튜닝 비교](../../comparisons/model-tuning.md) | 라우팅을 명시할 수 있지만 fallback 품질 관리가 필요하다. |

### 2. 설계안 요약

- 목표: 작은 문서 생성 agent가 “초안 작성 → 링크 검증 → 완료 receipt”를 수행한다.
- Non-goals: 브라우저 UI, 다중 모델 routing, 자동 배포.
- 도구 표면: Markdown 파일 읽기/쓰기, 링크 검사 스크립트만 허용.
- 루프: draft → validate links → revise once → receipt.
- prompt contract: 완료 전 링크 검사 결과를 반드시 보고한다.
- 아키텍처: 단일 CLI + `./.toy-ledger.jsonl`.
- 모델 라우팅: 단일 모델만 사용하고 실패 시 중단한다.
- 검증 계획: 링크가 깨진 fixture와 정상 fixture를 모두 테스트한다.

### 3. Toy 구현 계획 요약

```text
toy-doc-agent/
├─ README.md
├─ src/cli.ts
├─ src/loop.ts
├─ src/link-check.ts
└─ tests/link-check.test.ts
```

- 정상 경로: 유효한 Markdown 링크를 가진 문서를 받아 receipt를 남긴다.
- 실패 경로: 깨진 링크가 있으면 완료하지 않고 blocker를 출력한다.
- 검증: `tests/link-check.test.ts`에서 정상/실패 fixture를 모두 실행한다.

## 루브릭 대응 메모

- 분석 정확성: 5개 차원 모두 기존 문서 링크로 뒷받침됨.
- 설계 일관성: 루프와 도구 표면이 “문서 생성+링크 검증” 목표에 맞게 축소됨.
- 근거 링크: 모든 핵심 결정이 `gajae-code`, 비교 문서, 챕터 링크를 가짐.
- 구현 가능성: 파일 5개 이하, 테스트 fixture 2개로 작게 제한됨.

## 관련 링크

- [capstone 안내](../chapters/capstone.md)
- [분석 보고서 템플릿](analysis-report-template.md)
- [설계안 템플릿](design-proposal-template.md)
- [toy 구현 계획 템플릿](toy-implementation-plan-template.md)
- [루브릭](rubric.md)

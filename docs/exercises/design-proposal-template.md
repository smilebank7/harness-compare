# 설계안 템플릿

## 목적

이 템플릿은 capstone 2단계 산출물입니다. 분석 보고서에서 추출한 패턴과 tradeoff를 바탕으로 새 AI 코딩 하네스의 설계 결정을 명시합니다.

## 사용 방법

1. [분석 보고서 템플릿](analysis-report-template.md)의 패턴 추출 결과를 가져옵니다.
2. 목표와 non-goals를 먼저 정합니다.
3. 도구 표면, 루프, 프롬프트, 아키텍처, 모델 라우팅, 검증 계획을 각각 결정합니다.
4. 각 결정에 근거 링크 또는 명시적 가정을 붙입니다.

## 템플릿

### 1. 설계 목표

- 해결하려는 문제:
- 대상 사용자:
- 최종 산출물:
- 성공 기준:

### 2. Non-goals

- 하지 않을 것 1:
- 하지 않을 것 2:
- 하지 않을 것 3:

### 3. Decision drivers

| Driver | 왜 중요한가 | 관련 근거 |
|--------|-------------|-----------|
|  |  |  |
|  |  |  |
|  |  |  |

### 4. 도구 표면

- 제공할 읽기 도구:
- 제공할 변경 도구:
- 제공할 실행/검증 도구:
- 금지하거나 제한할 도구:
- 근거 링크:

### 5. 루프 설계

- 기본 루프:
- 재시도 조건:
- 종료 조건:
- checkpoint/receipt 방식:
- 근거 링크:

### 6. 프롬프트와 역할 지시

- system prompt 핵심 규칙:
- skill/workflow prompt:
- role agent가 필요한 경우:
- 금지 행동:
- 근거 링크:

### 7. 아키텍처

- 배포 형태(plugin, CLI, workflow OS 등):
- 상태 저장 위치:
- adapter/bridge 경계:
- audit trail 위치:
- 근거 링크:

### 8. 모델 라우팅

- routing signal:
- model profile:
- fallback chain:
- fallback 시 품질 제한:
- 근거 링크:

### 9. 검증 계획

- 파일/문서 검증:
- CLI/API/GUI/알고리즘 검증:
- red-team QA:
- 완료 기준:
- blocker 처리:

## 체크리스트

- [ ] 목표와 non-goals가 분리되어 있다.
- [ ] 5개 비교 차원과 연결되는 결정이 모두 있다.
- [ ] 각 결정에 근거 링크 또는 명시적 가정이 있다.
- [ ] fallback이 실패를 숨기지 않는다.
- [ ] 검증 계획이 완료 주장 범위와 일치한다.

## 관련 링크

- [capstone 안내](../chapters/capstone.md)
- [분석 보고서 템플릿](analysis-report-template.md)
- [toy 구현 계획 템플릿](toy-implementation-plan-template.md)
- [루브릭](rubric.md)
- [패턴 색인](../pattern-index.md)

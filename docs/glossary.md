# 용어집

이 용어집은 `harness-compare` 학습 레이어에서 반복되는 핵심 용어를 짧게 정의합니다. 각 항목은 관련 챕터와 기존 근거 문서로 돌아가는 링크를 포함합니다.

## 사용법

1. 처음 읽을 때는 [학습 경로](learning-path.md)의 2단계에서 주요 용어를 훑습니다.
2. 챕터를 읽다가 모르는 단어가 나오면 해당 항목의 관련 챕터와 기존 근거 링크를 함께 확인합니다.
3. 설계안을 쓸 때는 용어를 임의로 바꾸지 말고 이 문서의 이름을 기준으로 사용합니다.

## 용어 항목

### 1. 하네스

AI 모델이 실제 코딩 작업을 수행하도록 도구, 권한, 컨텍스트, 루프, 프롬프트, 상태를 묶어주는 실행 환경입니다.

- 관련 챕터: [map](chapters/map.md), [architecture](chapters/architecture.md)
- 기존 근거: [framework](../framework.md), [하네스 엔지니어링 비교](../comparisons/harness-engineering.md)

### 2. 하네스 엔지니어링

도구 표면, 권한 경계, 컨텍스트 공급, 파일/프로세스 접근처럼 모델이 행동할 수 있는 substrate를 설계하는 영역입니다.

- 관련 챕터: [tools](chapters/tools.md), [context](chapters/context.md)
- 기존 근거: [framework](../framework.md), [하네스 엔지니어링 비교](../comparisons/harness-engineering.md)

### 3. 루프 엔지니어링

모델 호출 이후 다음 행동을 결정하고, 검증·재시도·종료 조건을 관리하는 반복 제어 설계입니다.

- 관련 챕터: [loops](chapters/loops.md), [verification](chapters/verification.md)
- 기존 근거: [framework](../framework.md), [루프 엔지니어링 비교](../comparisons/loop-engineering.md)

### 4. 프롬프트 엔지니어링

역할, 금지/허용 행동, 도구 사용 원칙, 검증 의무를 자연어 또는 asset으로 모델에 주입하는 계약 설계입니다.

- 관련 챕터: [prompts](chapters/prompts.md), [mental-model](chapters/mental-model.md)
- 기존 근거: [framework](../framework.md), [프롬프트 엔지니어링 비교](../comparisons/prompt-engineering.md)

### 5. 아키텍처

하네스가 CLI, 플러그인, 서버, 브리지, 로컬 상태 디렉터리, 외부 런타임으로 나뉘는 배치와 경계입니다.

- 관련 챕터: [architecture](chapters/architecture.md)
- 기존 근거: [framework](../framework.md), [아키텍처 비교](../comparisons/architecture.md)

### 6. 모델별 튜닝

모델 ID, provider, capability, cost, latency, prompt 형식 차이에 따라 라우팅·fallback·adapter를 조정하는 설계입니다.

- 관련 챕터: [model-routing](chapters/model-routing.md)
- 기존 근거: [framework](../framework.md), [모델별 튜닝 비교](../comparisons/model-tuning.md)

### 7. 도구 표면

모델이 호출할 수 있는 파일 읽기, 검색, 편집, 셸, 브라우저, LSP, 서브에이전트 같은 조작 가능한 인터페이스의 집합입니다.

- 관련 챕터: [tools](chapters/tools.md)
- 기존 근거: [opencode 분석](../harnesses/opencode.md), [gajae-code 분석](../harnesses/gajae-code.md)

### 8. 권한 경계

도구가 어떤 파일, 명령, 네트워크, 상태에 접근할 수 있는지 제한하는 안전 경계입니다.

- 관련 챕터: [tools](chapters/tools.md), [architecture](chapters/architecture.md)
- 기존 근거: [framework](../framework.md), [하네스 엔지니어링 비교](../comparisons/harness-engineering.md)

### 9. 컨텍스트 공급

모델이 판단에 필요한 파일, 검색 결과, 상태, 사용자 의도, 이전 산출물을 언제 어떤 형태로 받는지 정하는 설계입니다.

- 관련 챕터: [context](chapters/context.md)
- 기존 근거: [context 관련 비교](../comparisons/harness-engineering.md), [ouroboros 분석](../harnesses/ouroboros.md)

### 10. 컨텍스트 압축

긴 히스토리나 대형 파일을 그대로 넣지 않고 요약·색인·선별로 모델 입력을 안정화하는 방식입니다.

- 관련 챕터: [context](chapters/context.md), [loops](chapters/loops.md)
- 기존 근거: [gajae-code 분석](../harnesses/gajae-code.md), [Ouroboros 분석](../harnesses/ouroboros.md)

### 11. 프롬프트 asset

파일 또는 번들 형태로 저장되어 모델 지시문에 재사용되는 역할·정책·예시 문서입니다.

- 관련 챕터: [prompts](chapters/prompts.md)
- 기존 근거: [opencode 분석](../harnesses/opencode.md), [프롬프트 엔지니어링 비교](../comparisons/prompt-engineering.md)

### 12. 역할 지시

Planner, Architect, Critic, Executor처럼 모델에게 특정 책임과 금지 행동을 부여하는 프롬프트 계약입니다.

- 관련 챕터: [prompts](chapters/prompts.md), [orchestration](chapters/orchestration.md)
- 기존 근거: [gajae-code 분석](../harnesses/gajae-code.md), [프롬프트 엔지니어링 비교](../comparisons/prompt-engineering.md)

### 13. 시스템 프롬프트

모델 행동의 기본 규칙을 결정하는 최상위 지시문입니다. 하네스 설계에서는 도구 우선순위, 안전 경계, 검증 의무를 담는 경우가 많습니다.

- 관련 챕터: [prompts](chapters/prompts.md)
- 기존 근거: [framework](../framework.md), [프롬프트 엔지니어링 비교](../comparisons/prompt-engineering.md)

### 14. 루프 종료 조건

작업을 계속할지 멈출지 판단하는 규칙입니다. 테스트 통과, 모호도 임계값, reviewer 승인, ledger checkpoint 등이 종료 조건이 될 수 있습니다.

- 관련 챕터: [loops](chapters/loops.md), [verification](chapters/verification.md)
- 기존 근거: [루프 엔지니어링 비교](../comparisons/loop-engineering.md), [ouroboros 분석](../harnesses/ouroboros.md)

### 15. 검증 게이트

완료 주장 전에 파일, 테스트, 리뷰, QA, 근거 링크를 확인하도록 강제하는 통과 조건입니다.

- 관련 챕터: [verification](chapters/verification.md), [loops](chapters/loops.md)
- 기존 근거: [gajae-code 분석](../harnesses/gajae-code.md), [루프 엔지니어링 비교](../comparisons/loop-engineering.md)

### 16. 레저

진행 상태, receipt, checkpoint, blocker, handoff를 append-only 또는 감사 가능한 형태로 기록하는 상태 장부입니다.

- 관련 챕터: [loops](chapters/loops.md), [architecture](chapters/architecture.md)
- 기존 근거: [gajae-code 분석](../harnesses/gajae-code.md), [루프 엔지니어링 비교](../comparisons/loop-engineering.md)

### 17. Receipt

특정 산출물이나 검증 결과가 언제 어떤 hash/path/evidence로 생성되었는지 요약하는 감사 가능한 증거 단위입니다.

- 관련 챕터: [verification](chapters/verification.md), [loops](chapters/loops.md)
- 기존 근거: [gajae-code 분석](../harnesses/gajae-code.md), [framework](../framework.md)

### 18. Checkpoint

긴 작업을 작은 목표 단위로 끊고 완료 증거와 함께 상태를 확정하는 지점입니다.

- 관련 챕터: [loops](chapters/loops.md), [capstone](chapters/capstone.md)
- 기존 근거: [gajae-code 분석](../harnesses/gajae-code.md), [루프 엔지니어링 비교](../comparisons/loop-engineering.md)

### 19. 서브에이전트

큰 작업을 역할별 또는 파일별로 나누어 수행하는 보조 agent입니다. 품질을 올릴 수 있지만 조율 비용과 race risk를 동반합니다.

- 관련 챕터: [orchestration](chapters/orchestration.md)
- 기존 근거: [omc 분석](../harnesses/omc.md), [gajae-code 분석](../harnesses/gajae-code.md)

### 20. 오케스트레이션

여러 agent, tool, workflow, review lane이 어떤 순서와 책임으로 협력하는지 정하는 조율 설계입니다.

- 관련 챕터: [orchestration](chapters/orchestration.md)
- 기존 근거: [아키텍처 비교](../comparisons/architecture.md), [omc 분석](../harnesses/omc.md)

### 21. Hook

특정 이벤트 전후에 실행되는 확장 지점입니다. 권한 검사, 상태 기록, 자동 검증, prompt injection에 쓰입니다.

- 관련 챕터: [tools](chapters/tools.md), [loops](chapters/loops.md)
- 기존 근거: [omc 분석](../harnesses/omc.md), [lazycodex 분석](../harnesses/lazycodex.md)

### 22. Workflow

여러 단계의 작업·검증·승인 절차를 이름 있는 경로로 묶은 것입니다.

- 관련 챕터: [loops](chapters/loops.md), [orchestration](chapters/orchestration.md)
- 기존 근거: [gajae-code 분석](../harnesses/gajae-code.md), [Ouroboros 분석](../harnesses/ouroboros.md)

### 23. Deep Interview

모호한 요구를 질문과 수치화된 모호도 점수로 명확한 spec으로 바꾸는 requirements workflow입니다.

- 관련 챕터: [capstone](chapters/capstone.md), [verification](chapters/verification.md)
- 기존 근거: [Ouroboros 분석](../harnesses/ouroboros.md), [gajae-code 분석](../harnesses/gajae-code.md)

### 24. Socratic questioning

사용자의 숨은 가정과 용어 불안정을 드러내기 위해 한 번에 하나의 질문으로 범위를 좁히는 방식입니다.

- 관련 챕터: [mental-model](chapters/mental-model.md), [capstone](chapters/capstone.md)
- 기존 근거: [Ouroboros 분석](../harnesses/ouroboros.md), [프롬프트 엔지니어링 비교](../comparisons/prompt-engineering.md)

### 25. Seed

Ouroboros류 시스템에서 요구·목표·초기 상태를 워크플로가 확장할 수 있는 입력 구조로 정리한 단위입니다.

- 관련 챕터: [context](chapters/context.md), [loops](chapters/loops.md)
- 기존 근거: [Ouroboros 분석](../harnesses/ouroboros.md), [루프 엔지니어링 비교](../comparisons/loop-engineering.md)

### 26. Evaluation gate

생성된 산출물이 기준을 만족하는지 평가하고 통과·재시도·중단을 결정하는 루프 게이트입니다.

- 관련 챕터: [verification](chapters/verification.md), [loops](chapters/loops.md)
- 기존 근거: [Ouroboros 분석](../harnesses/ouroboros.md), [루프 엔지니어링 비교](../comparisons/loop-engineering.md)

### 27. Evolution gate

평가 결과를 바탕으로 스펙, 계획, 구현 방향을 갱신할지 결정하는 진화형 루프의 경계입니다.

- 관련 챕터: [loops](chapters/loops.md), [architecture](chapters/architecture.md)
- 기존 근거: [Ouroboros 분석](../harnesses/ouroboros.md), [아키텍처 비교](../comparisons/architecture.md)

### 28. 모델 라우팅

요청 특성, provider, 모델 능력, 비용, 실패 상황에 따라 어떤 모델을 호출할지 고르는 분기 로직입니다.

- 관련 챕터: [model-routing](chapters/model-routing.md)
- 기존 근거: [모델별 튜닝 비교](../comparisons/model-tuning.md), [gajae-code 분석](../harnesses/gajae-code.md)

### 29. Fallback

선택한 모델, 도구, agent, 경로가 실패했을 때 대체 경로를 선택하는 설계입니다.

- 관련 챕터: [model-routing](chapters/model-routing.md), [verification](chapters/verification.md)
- 기존 근거: [모델별 튜닝 비교](../comparisons/model-tuning.md), [omo 분석](../harnesses/omo.md)

### 30. Adapter

host, provider, CLI, SDK 차이를 공통 인터페이스 뒤로 숨기는 변환 계층입니다.

- 관련 챕터: [architecture](chapters/architecture.md), [model-routing](chapters/model-routing.md)
- 기존 근거: [아키텍처 비교](../comparisons/architecture.md), [omo 분석](../harnesses/omo.md)

### 31. Permalink

분석 시점의 고정 SHA와 line range를 포함해 나중에 upstream이 바뀌어도 근거를 재확인할 수 있는 링크입니다.

- 관련 챕터: [verification](chapters/verification.md)
- 기존 근거: [framework](../framework.md), [README 검증 기준](../README.md#검증-기준)

### 32. AC3a

문서의 모든 GitHub 소스 permalink가 고정 SHA, 경로, line range를 실제로 가리키는지 기계적으로 확인하는 검증입니다.

- 관련 챕터: [verification](chapters/verification.md)
- 기존 근거: [framework](../framework.md), [README 검증 기준](../README.md#검증-기준)

### 33. AC3b

기계 검증만으로는 부족한 주장 왜곡 여부를 표본 검토로 확인하는 품질 검증입니다.

- 관련 챕터: [verification](chapters/verification.md), [mental-model](chapters/mental-model.md)
- 기존 근거: [framework](../framework.md), [README 검증 기준](../README.md#검증-기준)

### 34. 가치중립 명사구

좋다/나쁘다 판단을 피하고 설계 접근을 이름 붙이는 README 매트릭스 셀의 표현 방식입니다.

- 관련 챕터: [mental-model](chapters/mental-model.md), [map](chapters/map.md)
- 기존 근거: [README 요약 매트릭스](../README.md#요약-매트릭스), [framework](../framework.md)

### 35. 기준선

다른 하네스와의 차이를 설명하기 위해 비교 축의 출발점으로 삼는 문서나 시스템입니다. 이 레포에서는 opencode가 기준선 역할을 합니다.

- 관련 챕터: [map](chapters/map.md), [mental-model](chapters/mental-model.md)
- 기존 근거: [opencode 분석](../harnesses/opencode.md), [README 분석 대상](../README.md#분석-대상)

### 36. 플러그인형 하네스

기존 host/CLI/SDK 위에 추가 도구, prompt, hook, workflow를 얹어 기능을 확장하는 형태입니다.

- 관련 챕터: [architecture](chapters/architecture.md), [tools](chapters/tools.md)
- 기존 근거: [omo 분석](../harnesses/omo.md), [lazycodex 분석](../harnesses/lazycodex.md)

### 37. 독립형 하네스

host에 붙는 플러그인보다 자체 CLI, 상태 디렉터리, workflow runtime, tool surface를 중심으로 설계되는 형태입니다.

- 관련 챕터: [architecture](chapters/architecture.md), [orchestration](chapters/orchestration.md)
- 기존 근거: [gajae-code 분석](../harnesses/gajae-code.md), [아키텍처 비교](../comparisons/architecture.md)

### 38. 스펙 주도 루프

사용자 의도를 먼저 명확한 spec으로 고정하고, 계획·생성·평가·진화를 그 spec에 종속시키는 루프 방식입니다.

- 관련 챕터: [loops](chapters/loops.md), [capstone](chapters/capstone.md)
- 기존 근거: [Ouroboros 분석](../harnesses/ouroboros.md), [루프 엔지니어링 비교](../comparisons/loop-engineering.md)

### 39. Completion claim

agent가 “끝났다”고 말하는 주장입니다. 이 레포의 학습 관점에서는 항상 파일·검증·근거 링크 같은 직접 증거와 함께 다뤄야 합니다.

- 관련 챕터: [verification](chapters/verification.md)
- 기존 근거: [gajae-code 분석](../harnesses/gajae-code.md), [README 검증 기준](../README.md#검증-기준)

### 40. Red-team QA

완성물을 확인만 하는 것이 아니라 의도적으로 깨뜨리거나 누락을 찾는 검증 관점입니다.

- 관련 챕터: [verification](chapters/verification.md), [capstone](chapters/capstone.md)
- 기존 근거: [gajae-code 분석](../harnesses/gajae-code.md), [루프 엔지니어링 비교](../comparisons/loop-engineering.md)

## Backlinks

- [학습 경로](learning-path.md)
- [문서 맵](document-map.md)
- [개념 색인](concept-index.md)
- [패턴 색인](pattern-index.md)
- [framework](../framework.md)

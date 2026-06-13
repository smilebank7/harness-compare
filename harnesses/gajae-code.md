# Gajae-Code / GJC 학습 위키

- **정식 레포명:** Yeachan-Heo/gajae-code (약칭: gajae-code, gjc)
- **분석 기준 커밋:** `0464f0f92a278ed34059db48d9951cbf41c1ce78`
- **클론 채록일:** 2026-06-12 / **분석일:** 2026-06-12
- **분석 축:** 독립형 외부 coding-agent harness

이 문서는 Gajae-Code를 처음 읽는 사람을 위한 학습형 위키입니다. 목표는 “이 하네스가 무엇을 해결하고, 어떤 층으로 나뉘며, 어디까지 근거가 확인됐는가”를 차근차근 따라가게 하는 것입니다. 모든 설계 설명은 고정 커밋의 GitHub permalink를 근거로 삼습니다.

## 읽는 방법

처음부터 끝까지 읽어도 되지만, 다음 순서가 가장 편합니다.

1. **먼저 읽는 요약**에서 GJC의 정체성을 잡습니다.
2. **개요**에서 전체 workflow 흐름을 봅니다.
3. 관심 있는 축으로 이동합니다: 하네스, 루프, 프롬프트, 아키텍처, 모델 튜닝.
4. 각 축의 “먼저 이해할 점”을 읽고, 필요할 때만 근거 링크를 따라갑니다.

> 빠르게 판단하려면 요약과 설계 결정 요약만 읽어도 됩니다. 구현 근거까지 확인하려면 각 문단의 permalink를 따라가면 됩니다.

## 이어 읽을 문서

이 문서는 GJC 한 하네스를 깊게 읽는 페이지입니다. 비교 관점은 아래 문서로 이어집니다.

- [전체 분석 프레임워크](../framework.md): 다섯 분석 축의 기준을 먼저 정의합니다.
- [하네스 엔지니어링 비교](../comparisons/harness-engineering.md): 다른 하네스와 도구·세션 경계를 비교합니다.
- [루프 엔지니어링 비교](../comparisons/loop-engineering.md): 질문, 계획, 실행, 검증 루프를 비교합니다.
- [프롬프트 엔지니어링 비교](../comparisons/prompt-engineering.md): skill prompt와 role prompt 설계를 비교합니다.
- [아키텍처 비교](../comparisons/architecture.md): runner, state, CLI, subagent 구조를 비교합니다.
- [모델 튜닝 비교](../comparisons/model-tuning.md): provider, model profile, fallback 정책을 비교합니다.

## 먼저 읽는 요약

Gajae-Code, 줄여서 GJC는 Claude Code나 Codex CLI 안에 끼워 넣는 플러그인이 아닙니다. 별도의 `gjc` 프로세스가 실행되고, 작업 중인 레포 옆에 `.gjc` 상태를 만들며, 요구사항 인터뷰부터 계획, 실행, 검증까지 자체 workflow로 관리합니다.

핵심을 한 문장으로 말하면 이렇습니다.

> **GJC는 “외부 runner + workflow OS + 검증 ledger”를 한데 묶은 코딩 에이전트 하네스입니다.**

깊게 읽기 전에 세 가지를 먼저 기억하면 됩니다.

1. **외부 runner**: Claude Code나 Codex CLI의 플러그인이 아니라, `gjc` CLI가 직접 세션과 도구를 구성합니다.
2. **작은 workflow 표면**: 공개 workflow는 `deep-interview`, `ralplan`, `ultragoal`, `team` 네 개로 제한됩니다.
3. **검증 중심 종료 조건**: “끝났다”는 말은 감상이 아니라 `.gjc` ledger, receipt, quality gate를 통과해야 성립합니다.

## 개요

README는 GJC를 “interviews, reviewed plans, tmux-native execution, and durable verification”을 수행하는 coding-agent runner라고 소개합니다. 동시에 beta-stage라서 출력 검증이 필요하다고 밝힙니다.

- 근거: [README.md#L5-L10](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/README.md#L5-L10), [README.md#L22-L22](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/README.md#L22-L22)

전체 흐름은 아래처럼 읽으면 됩니다.

```text
아이디어 정리
  → deep-interview
  → ralplan
  → ultragoal 또는 team
  → 검증 receipt
```

빠른 시작 명령은 `gjc`, `gjc --tmux`, `gjc --tmux --worktree`입니다. 세션 안에서는 `/skill:deep-interview`, `/skill:ralplan`, `gjc ultragoal create-goals`, `gjc ultragoal complete-goals` 같은 workflow가 예시로 제시됩니다. 설치 표면은 `bun install -g gajae-code`, scoped package `@gajae-code/coding-agent`, bin `gjc`로 정리됩니다.

- 근거: [README.md#L43-L65](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/README.md#L43-L65), [README.md#L35-L41](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/README.md#L35-L41), [packages/coding-agent/package.json#L28-L32](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/package.json#L28-L32)

GJC가 다른 하네스와 달라지는 지점도 README에서 확인할 수 있습니다. GJC는 Codex CLI, Claude Code, OpenCode, Claw Code의 extension으로 들어가지 않습니다. AGENTS.md도 같은 방향으로 라우팅 규칙을 둡니다. 낮은 위험 작업은 직접 처리할 수 있지만, 모호한 요구사항은 `deep-interview`, 계획 위험은 `ralplan`, 장기 실행은 `ultragoal`, 병렬 tmux 실행은 `team`으로 보냅니다.

- 근거: [README.md#L97-L106](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/README.md#L97-L106), [AGENTS.md#L32-L44](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/AGENTS.md#L32-L44)

### 한눈에 보는 구조

| 층 | GJC에서 하는 일 | 대표 근거 |
|---|---|---|
| CLI/runner | `gjc` 프로세스가 세션을 시작하고 도구를 구성 | [package.json#L28-L32](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/package.json#L28-L32) |
| ToolSession | read, bash, edit, search, lsp, browser, task, skill, goal 등을 묶음 | [tools/index.ts#L313-L348](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/tools/index.ts#L313-L348) |
| Workflow skill | deep-interview, ralplan, ultragoal, team 네 흐름을 모델 지시문으로 제공 | [README.md#L75-L84](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/README.md#L75-L84) |
| Role agent | executor, architect, planner, critic 역할을 분리 | [README.md#L86-L95](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/README.md#L86-L95) |
| Runtime state | spec, plan, goal ledger, team state를 `.gjc` 아래 저장 | [AGENTS.md#L23-L30](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/AGENTS.md#L23-L30) |


### 자주 나오는 용어

| 용어 | 이 문서에서의 뜻 |
|---|---|
| `gjc` | Gajae-Code를 실행하는 CLI와 그 세션 런타임 |
| Workflow skill | `deep-interview`, `ralplan`, `ultragoal`, `team`처럼 작업 흐름을 정의한 Markdown 기반 지시문 |
| Role agent | `planner`, `architect`, `critic`, `executor`처럼 한 역할에 맞춰 제한된 보조 에이전트 |
| `.gjc` | spec, plan, ledger, team state 같은 실행 산출물이 저장되는 runtime 상태 디렉터리 |
| Receipt | 특정 작업이나 검증이 완료됐다는 증거 기록 |
| Quality gate | 완료 선언 전에 통과해야 하는 검토·QA·재실행 조건 |

## 하네스 엔지니어링

framework.md 정의: [하네스 엔지니어링](../framework.md#1-하네스-엔지니어링-harness-engineering).

### 먼저 이해할 점

GJC의 하네스 엔지니어링은 **도구를 많이 붙이는 것**보다 **도구 호출이 일어나는 세션 경계**를 설계하는 데 가깝습니다. 모델이 읽고, 고치고, 검색하고, 브라우저를 열고, subagent를 부르고, 목표를 완료하는 모든 행위가 하나의 `ToolSession` 안에서 관리됩니다.

### 1. 도구 표면은 GJC가 직접 만든다

`tools/index.ts`는 세션에서 사용할 도구를 한곳에 모읍니다. Ask, AST Grep/Edit, Bash, Browser, Debug, Eval, Find, GitHub, IRC, Job, Monitor, Read, Resolve, Search, Skill, Subagent, Todo, Write, LSP, Goal, Task, WebSearch 계열이 이 층에서 구성됩니다.

- 근거: [packages/coding-agent/src/tools/index.ts#L32-L61](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/tools/index.ts#L32-L61)

공개 callable map도 같은 파일에서 만들어집니다. 여기에는 `read`, `bash`, `edit`, `ast_grep`, `ask`, `lsp`, `browser`, `task`, `subagent`, `skill`, `goal` 등이 들어갑니다. 기본 essential 도구는 `read`, `bash`, `edit`입니다.

- 근거: [packages/coding-agent/src/tools/index.ts#L313-L348](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/tools/index.ts#L313-L348), [packages/coding-agent/src/tools/index.ts#L289-L304](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/tools/index.ts#L289-L304)

읽는 포인트는 간단합니다. GJC는 특정 IDE나 플러그인 시스템에 도구를 위임하지 않습니다. GJC 프로세스가 직접 “모델에게 어떤 도구를 줄지”를 결정합니다.

### 2. ToolSession은 작업 문맥의 묶음이다

`ToolSession`에는 cwd, UI 여부, workspace tree, active skill, LSP, event bus, output schema, task depth, session file, artifact manager, model/auth/settings, goal runtime, todo phases 등이 들어갑니다([packages/coding-agent/src/tools/index.ts#L119-L240](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/tools/index.ts#L119-L240)).

즉, 도구 호출은 단순 함수 호출이 아닙니다. “어느 레포에서, 어떤 skill phase에서, 어떤 목표 상태로, 어떤 모델 설정을 들고 호출했는가”가 함께 따라갑니다.

### 3. Read/Edit/Bash도 하네스 장치다

`read.ts`는 파일을 여는 도구 이상입니다. sqlite, glob/code summary, image metadata, notebook, archive, URL, conflict detection, document conversion까지 포함합니다. 코드 summary에는 line/byte 상한과 elision footer가 있어, 모델이 필요한 범위를 다시 읽을 수 있게 합니다.

- 근거: [packages/coding-agent/src/tools/read.ts#L1-L90](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/tools/read.ts#L1-L90), [packages/coding-agent/src/tools/read.ts#L183-L194](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/tools/read.ts#L183-L194)

`edit/index.ts`는 replace, patch, hashline, vim, apply_patch 문법과 LSP format/diagnostics writethrough를 한 도구 안에 묶습니다. 여러 파일 편집에서 일부가 실패하면 aggregate result를 error branch로 넘겨 “성공처럼 보이는 실패”를 막습니다.

- 근거: [packages/coding-agent/src/edit/index.ts#L1-L57](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/edit/index.ts#L1-L57), [packages/coding-agent/src/edit/index.ts#L72-L118](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/edit/index.ts#L72-L118), [packages/coding-agent/src/edit/index.ts#L258-L272](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/edit/index.ts#L258-L272)

Hashline 편집은 stale-write 방지 장치입니다. anchor hash가 맞지 않으면 자동 보정하지 않고 `HashlineMismatchError`로 실패를 명확하게 보여줍니다([packages/coding-agent/src/hashline/apply.ts#L45-L66](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/hashline/apply.ts#L45-L66)).

`bash.ts`는 실행 기능만이 아니라 restricted role-agent bash, internal URL expansion, streaming tail output, command fixups, timeout clamp 등을 함께 다룹니다([packages/coding-agent/src/tools/bash.ts#L1-L34](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/tools/bash.ts#L1-L34)).

### 4. Role-agent 실행도 하네스 표면이다

Task tool은 bundled/user/project agent definition을 발견하고, single/parallel/progress/artifact 실행을 지원합니다([packages/coding-agent/src/task/index.ts#L1-L14](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/task/index.ts#L1-L14)). `task/agents.ts`는 executor, architect, planner, critic 등을 source Markdown에서 불러와 `AgentDefinition`으로 만듭니다([packages/coding-agent/src/task/agents.ts#L1-L68](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/task/agents.ts#L1-L68), [packages/coding-agent/src/task/agents.ts#L95-L164](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/task/agents.ts#L95-L164)).

여기서 중요한 점은 role agent도 “별도 봇”이 아니라 GJC 세션 권한과 artifact 체계 안에서 실행된다는 점입니다.

## 루프 엔지니어링

framework.md 정의: [루프 엔지니어링](../framework.md#2-루프-엔지니어링-loop-engineering).

### 먼저 이해할 점

GJC의 루프는 “모델이 계속 시도한다”가 아닙니다. 각 단계가 **질문 → 계획 → 검토 → 실행 → 검증**으로 분리되고, 다음 단계로 넘어가는 조건이 명시됩니다.

### 1. Deep Interview: 모호하면 실행하지 않는다

Deep Interview는 숨은 가정을 질문으로 확인하고, weighted ambiguity가 threshold 아래로 내려갈 때까지 실행을 막습니다([packages/coding-agent/src/defaults/gjc/skills/deep-interview/SKILL.md#L13-L15](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/deep-interview/SKILL.md#L13-L15)).

운영 규칙은 꽤 엄격합니다.

- 질문은 한 번에 하나만 합니다.
- 가장 약한 clarity dimension을 겨냥합니다.
- Round 0에서 top-level component를 먼저 확정합니다.
- 답변마다 ambiguity를 다시 계산합니다.
- 여러 component가 있으면 component별로 점수를 냅니다.
- 명시 승인 전에는 실행으로 넘어가지 않습니다.

근거: [deep-interview/SKILL.md#L40-L55](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/deep-interview/SKILL.md#L40-L55)

Round 0 topology gate는 특히 중요합니다. 상세하게 말한 한 부분에만 질문이 몰리는 문제를 막기 위해, 먼저 scope의 모양을 1~6개 component로 고정합니다([deep-interview/SKILL.md#L168-L176](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/deep-interview/SKILL.md#L168-L176)). 이후 질문 루프는 가장 낮은 component/dimension pair를 고르고, 동률이면 component를 회전시킵니다([deep-interview/SKILL.md#L237-L255](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/deep-interview/SKILL.md#L237-L255)).

### 2. Ralplan: 계획은 합의될 때까지 재검토한다

Ralplan은 Planner → Architect → Critic 순서로 계획을 검토합니다. interactive flag가 없으면 자동으로 consensus loop를 돌리고, 최종 계획을 `pending approval`로 표시한 뒤 실행하지 않고 멈춥니다([ralplan/SKILL.md#L20-L27](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/ralplan/SKILL.md#L20-L27)).

Planning/Execution Boundary도 분명합니다. 명시 승인 전에는 source edit, commit, push, execution skill 호출, implementation delegation이 금지됩니다([ralplan/SKILL.md#L36-L46](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/ralplan/SKILL.md#L36-L46)).

루프 구조는 다음과 같습니다.

```text
Planner가 초안 작성
  → Architect가 구조 검토
  → Critic이 실행 가능성 검토
  → 미승인 시 같은 Planner를 resume해 수정
  → 최대 5회 반복
```

근거: [ralplan/SKILL.md#L56-L89](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/ralplan/SKILL.md#L56-L89)

Native `gjc ralplan` runtime은 이 판단 루프를 직접 실행하는 엔진이라기보다, state와 artifact를 안전하게 저장하는 경계입니다. 코드 주석도 loop 자체는 bundled `/skill:ralplan`에 있다고 설명합니다([ralplan-runtime.ts#L12-L29](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/gjc-runtime/ralplan-runtime.ts#L12-L29)).

### 3. Skill handoff: 다음 skill로 넘어가려면 phase가 맞아야 한다

`tools/skill.ts`는 skill chaining을 지원하지만, caller의 `current_phase`가 `complete`, `handoff`, `failed`, `cancelled`, `inactive` 같은 허용 상태가 아니면 거부합니다([tools/skill.ts#L1-L16](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/tools/skill.ts#L1-L16), [tools/skill.ts#L29-L34](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/tools/skill.ts#L29-L34)).

즉, “다음 프롬프트를 붙인다”가 아니라 workflow phase transition이 성공해야 다음 루프가 열립니다. 실제 실행부도 `gjc state <caller> handoff --to <callee>`를 in-process로 수행한 뒤 다음 skill 지시문을 dispatch합니다([tools/skill.ts#L90-L151](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/tools/skill.ts#L90-L151)).

### 4. Ultragoal: 완료는 receipt로 증명한다

Ultragoal은 brief, plan, checkpoint audit를 `.gjc/ultragoal/` 아래에 둡니다([ultragoal/SKILL.md#L12-L20](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/ultragoal/SKILL.md#L12-L20)). Complete-goals loop는 story 수행, completion audit, cleanup/review gate, fresh goal snapshot, checkpoint receipt 순서로 진행됩니다([ultragoal/SKILL.md#L86-L104](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/ultragoal/SKILL.md#L86-L104)).

Native runtime은 receipt freshness와 quality gate 구조를 TypeScript 코드로 검사합니다. Complete checkpoint에는 `architectReview`, `executorQa`, `iteration`이 필요하고, status가 모두 clean이어야 합니다([ultragoal-runtime.ts#L996-L1062](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/gjc-runtime/ultragoal-runtime.ts#L996-L1062)).

Goal tool의 `complete`도 같은 방향입니다. 도구는 `create`, `get`, `complete`, `resume`, `drop`만 허용하고, 완료 직전에 `assertCanCompleteCurrentGoal`을 호출합니다([goal-tool.ts#L18-L25](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/goals/tools/goal-tool.ts#L18-L25), [goal-tool.ts#L69-L99](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/goals/tools/goal-tool.ts#L69-L99)).

## 프롬프트 엔지니어링

framework.md 정의: [프롬프트 엔지니어링](../framework.md#3-프롬프트-엔지니어링-prompt-engineering).

### 먼저 이해할 점

GJC의 프롬프트 엔지니어링은 “좋은 말투를 써라”보다 더 구조적입니다. workflow skill과 role agent의 Markdown이 모델에게 들어가는 계약 문서이고, 이 문서들이 실행 경계와 검증 책임을 나눕니다.

### 1. 공개 workflow는 source-bundled Markdown이다

README는 default workflow definition이 `.gjc` 복사본이 아니라 source path에 있다고 설명합니다. 경로는 `packages/coding-agent/src/defaults/gjc/skills/<name>/SKILL.md`와 `packages/coding-agent/src/prompts/agents/<role>.md`입니다([README.md#L141-L146](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/README.md#L141-L146)). AGENTS.md도 네 workflow skill의 source path를 표로 고정합니다([AGENTS.md#L5-L15](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/AGENTS.md#L5-L15)).

Deep Interview frontmatter는 name, description, argument hint, pipeline, handoff policy, handoff path를 갖습니다([deep-interview/SKILL.md#L1-L11](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/deep-interview/SKILL.md#L1-L11)). Ralplan frontmatter도 name, description, flags hint, source를 갖습니다([ralplan/SKILL.md#L1-L12](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/ralplan/SKILL.md#L1-L12)).

### 2. Skill prompt는 모델 행동을 절차로 묶는다

Deep Interview prompt는 Socratic questioning, mathematical ambiguity scoring, hidden assumption exposure, gated pipeline을 목적에 둡니다([deep-interview/SKILL.md#L13-L15](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/deep-interview/SKILL.md#L13-L15)). Use/Do-not-use 규칙은 vague idea, “ask me everything”, “don’t assume” 같은 trigger와 quick fix, 상세 요청, 기존 PRD 실행 요청을 분리합니다([deep-interview/SKILL.md#L17-L32](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/deep-interview/SKILL.md#L17-L32)).

Ralplan prompt는 consensus planning과 artifact discipline을 강제합니다. Planning artifact는 직접 `.gjc`를 고치는 대신 `gjc ralplan --write --stage ... --artifact ...`로 저장해야 합니다([ralplan/SKILL.md#L40-L52](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/ralplan/SKILL.md#L40-L52)). Planner, Architect, Critic output은 receipt/path와 compact verdict만 parent에게 돌려주도록 되어 있습니다([ralplan/SKILL.md#L56-L68](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/ralplan/SKILL.md#L56-L68)).

Ultragoal prompt는 goal tool 사용법과 completion gate를 계약으로 둡니다. Agent-facing goal surface는 `goal({"op":"get"})`, `create`, `complete`, `drop`, `resume`로 제한됩니다([ultragoal/SKILL.md#L38-L49](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/ultragoal/SKILL.md#L38-L49)). Mandatory completion gate는 cleanup sweep, architect review, executor QA/red-team, real surface evidence, strict quality gate, blocker recording, fresh snapshot checkpoint까지 요구합니다([ultragoal/SKILL.md#L175-L201](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/ultragoal/SKILL.md#L175-L201)).

Team prompt는 tmux-backed multi-worker execution mode를 정의하고, `.gjc/state/team/...` 파일과 `gjc team api ...`를 coordination surface로 둡니다([team/SKILL.md#L8-L18](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/team/SKILL.md#L8-L18)).

### 3. Role-agent prompt는 역할별 책임을 좁힌다

Role agent도 Markdown prompt로 정의됩니다.

| Role | 핵심 책임 | 근거 |
|---|---|---|
| Planner | 읽기 전용 계획 작성, durable plan 저장 | [planner.md#L1-L21](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/prompts/agents/planner.md#L1-L21) |
| Architect | 구조 검토, CLEAR/WATCH/BLOCK, verdict 제공 | [architect.md#L1-L34](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/prompts/agents/architect.md#L1-L34) |
| Executor | 구현 가능, 작은 diff, focused checks, QA matrix | [executor.md#L1-L31](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/prompts/agents/executor.md#L1-L31) |
| Critic | 계획 실행 가능성 판정, OKAY/ITERATE/REJECT | [critic.md#L1-L26](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/prompts/agents/critic.md#L1-L26) |

이 분리는 prompt를 “성격 설정”이 아니라 “권한과 검토 책임”으로 쓰는 방식입니다.

### 4. System prompt 조립도 코드화되어 있다

`system-prompt.ts`는 system prompt construction, project context loading, skill loading, prompt template, workspace tree를 조립합니다([system-prompt.ts#L1-L18](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/system-prompt.ts#L1-L18)). Context file dedupe는 exact content를 비교해 가까운 context entry를 남깁니다([system-prompt.ts#L232-L275](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/system-prompt.ts#L232-L275)). Project-level SYSTEM.md가 user-level SYSTEM.md보다 앞서 선택되는 구조도 있습니다([system-prompt.ts#L277-L295](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/system-prompt.ts#L277-L295)).

## 아키텍처

framework.md 정의: [아키텍처](../framework.md#4-아키텍처-architecture).

### 먼저 이해할 점

GJC의 아키텍처는 “플러그인 묶음”보다 “작은 공개 workflow를 가진 독립 CLI 제품”에 가깝습니다. source default와 runtime state를 분리하고, legacy command surface를 줄이며, subagent와 team runtime을 자체 상태 모델로 다룹니다.

### 1. 배포 단위는 monorepo와 `gjc` CLI다

Root `package.json`은 workspaces를 `packages/*`와 `python/robogjc/web`로 둡니다. catalog에는 `@gajae-code/stats`, `agent-core`, `ai`, `bridge-client`, `coding-agent`, `natives`, `tui`, `utils`가 묶입니다([package.json#L1-L31](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/package.json#L1-L31)). Scripts는 `dev`, `build`, `test`, `check`, `lint`, `fmt`, `ci`, release, model generation, UI verification 등을 포함합니다([package.json#L80-L156](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/package.json#L80-L156)).

`@gajae-code/coding-agent` package는 CLI application 경계입니다. Bin은 `gjc: src/cli.ts`로 잡혀 있습니다([packages/coding-agent/package.json#L28-L32](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/package.json#L28-L32)).

### 2. Runtime state는 `.gjc` 아래에 둔다

AGENTS.md는 runtime state, plans, specs, workflow ledgers가 `.gjc/`에 속한다고 규정합니다([AGENTS.md#L23-L30](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/AGENTS.md#L23-L30)). README도 workflow definition의 원본은 source path에 있고, committed `.gjc` copy가 SSOT가 아니라고 설명합니다([README.md#L141-L146](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/README.md#L141-L146)).

Ultragoal runtime은 `.gjc/ultragoal/brief.md`, `goals.json`, `ledger.jsonl` 경로를 계산합니다([ultragoal-runtime.ts#L139-L147](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/gjc-runtime/ultragoal-runtime.ts#L139-L147)). Team runtime은 `.gjc/state/team`을 state root로 삼습니다([team/SKILL.md#L141-L150](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/team/SKILL.md#L141-L150)).

### 3. 공개 표면을 작게 유지한다

`packages/coding-agent/package.json` exports는 여러 legacy 또는 부가 command surface를 `null`로 닫습니다. 예를 들어 MCP helper, autoresearch, ralph, ultraqa, ultrawork, performance-goal 관련 path가 막혀 있습니다([packages/coding-agent/package.json#L88-L115](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/package.json#L88-L115), [packages/coding-agent/package.json#L140-L147](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/package.json#L140-L147)).

README와 AGENTS가 공개 workflow를 네 개로 고정하는 점과 함께 보면, GJC는 기능을 많이 늘리는 대신 좁은 workflow surface를 유지하려는 설계를 택합니다([README.md#L75-L95](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/README.md#L75-L95), [AGENTS.md#L5-L21](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/AGENTS.md#L5-L21)).

### 4. Subagent와 Team도 runtime state로 관리한다

`task/executor.ts`는 subagent를 main thread에서 실행하고, AgentEvents를 progress tracking으로 전달합니다([task/executor.ts#L1-L5](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/task/executor.ts#L1-L5)). Parent ArtifactManager를 받아 artifact ID가 parent와 subagent 전체에서 충돌하지 않게 하는 구조도 있습니다([task/executor.ts#L150-L156](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/task/executor.ts#L150-L156)).

Team runtime은 tmux pane과 worktree integration을 상태로 모델링합니다. Worker lifecycle, task evidence, worktree info, team state root가 타입으로 잡힙니다([team-runtime.ts#L22-L33](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/gjc-runtime/team-runtime.ts#L22-L33), [team-runtime.ts#L49-L93](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/gjc-runtime/team-runtime.ts#L49-L93)).

## 모델별 튜닝

framework.md 정의: [모델별 튜닝](../framework.md#5-모델별-튜닝-per-model-tuning).

### 먼저 이해할 점

GJC의 모델 튜닝은 “모델 이름 하나 고르기”에 그치지 않습니다. Provider/model selector, thinking-level suffix, role-agent별 profile, OpenRouter fallback 후보 탐색이 함께 있습니다.

### 1. Provider/model selector와 thinking suffix

`model-resolver.ts`는 `provider/modelId` 형식의 model string을 파싱합니다. `:high` 같은 thinking-level suffix가 유효하면 model id와 thinking level을 분리합니다([model-resolver.ts#L35-L56](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/config/model-resolver.ts#L35-L56)). Format helper도 selector와 thinking level을 다시 합쳐 `provider/model:level` 형태로 만듭니다([model-resolver.ts#L58-L67](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/config/model-resolver.ts#L58-L67)).

### 2. OpenRouter는 별도 fallback 후보 탐색이 있다

Resolver는 routed suffix와 date suffix를 벗긴 OpenRouter candidate queue를 만듭니다([model-resolver.ts#L69-L113](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/config/model-resolver.ts#L69-L113)). Provider/model resolution에서 normalized provider가 `openrouter`가 아니면 exact match 실패 뒤 바로 undefined를 반환합니다([model-resolver.ts#L145-L180](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/config/model-resolver.ts#L145-L180)).

Bare model id가 모호할 때는 usage order, provider usage, vision-capable variant, deprioritized provider, model order를 순서대로 비교합니다([model-resolver.ts#L197-L267](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/config/model-resolver.ts#L197-L267)).

### 3. Model profile은 역할별 selector를 분리한다

`model-profiles.ts`는 profile definition에 name, requiredProviders, modelMapping, source를 둡니다([model-profiles.ts#L45-L54](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/config/model-profiles.ts#L45-L54)). Built-in profile은 opencode-go, codex, opencode-go-codex 계열을 eco/standard/pro로 나누고, `default`, `executor`, `architect`, `planner`, `critic` selector를 따로 매핑합니다([model-profiles.ts#L56-L120](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/config/model-profiles.ts#L56-L120)).

Role-agent prompt frontmatter도 이 구조와 맞물립니다. Planner는 thinking-level medium, Architect/Critic은 high, Executor는 medium을 선언합니다([planner.md#L1-L8](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/prompts/agents/planner.md#L1-L8), [architect.md#L1-L10](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/prompts/agents/architect.md#L1-L10), [critic.md#L1-L8](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/prompts/agents/critic.md#L1-L8), [executor.md#L1-L6](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/prompts/agents/executor.md#L1-L6)).

### 4. Retry budget은 provider 운영 튜닝이다

README는 `~/.gjc/config.yml`에 `requestMaxRetries`, `streamMaxRetries`, `maxRetries`, `maxDelayMs`를 둔다고 설명합니다([README.md#L108-L118](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/README.md#L108-L118)). Request retry는 stream 이전, stream retry는 replay-safe transient stream failure에만 적용됩니다. Invalid auth, unsupported model/provider, malformed request, context overflow, user abort, permanent quota failure는 fail-fast입니다([README.md#L120-L120](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/README.md#L120-L120)).

이 항목은 모델 선택보다 낮은 층의 튜닝입니다. Provider stream failure mode에 따라 “다시 시도해도 되는 실패”와 “즉시 실패해야 하는 실패”를 나눕니다.

## 설계 결정 요약

| 번호 | 결정 | 의미 | 대표 근거 |
|---:|---|---|---|
| 1 | 외부 runner 정체성 | 플러그인에 포함되지 않고 별도 `gjc` 프로세스가 workflow를 소유 | [README.md#L24-L33](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/README.md#L24-L33), [README.md#L97-L106](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/README.md#L97-L106) |
| 2 | 작은 workflow surface | 공개 skill 네 개와 role agent 네 개로 사용 표면을 제한 | [README.md#L75-L95](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/README.md#L75-L95), [AGENTS.md#L5-L21](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/AGENTS.md#L5-L21) |
| 3 | source default와 runtime `.gjc` 분리 | 정의는 source에, 실행 상태는 `.gjc`에 저장 | [README.md#L141-L146](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/README.md#L141-L146), [AGENTS.md#L23-L30](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/AGENTS.md#L23-L30) |
| 4 | 도구형 하네스 | 파일, 편집, 실행, 검색, LSP, browser, subagent, skill, goal 도구를 세션 안에서 직접 구성 | [tools/index.ts#L313-L348](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/tools/index.ts#L313-L348) |
| 5 | hashline/LSP 편집 안전 | stale anchor를 자동 보정하지 않고 실패를 명확히 표시 | [edit/index.ts#L113-L118](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/edit/index.ts#L113-L118), [hashline/apply.ts#L45-L66](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/hashline/apply.ts#L45-L66) |
| 6 | 승인-gated skill handoff | phase guard를 통과해야 다음 workflow로 넘어감 | [tools/skill.ts#L90-L151](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/tools/skill.ts#L90-L151) |
| 7 | Ultragoal receipt gate | 완료는 quality gate와 fresh goal snapshot으로 증명 | [ultragoal/SKILL.md#L175-L201](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/ultragoal/SKILL.md#L175-L201), [ultragoal-runtime.ts#L996-L1062](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/gjc-runtime/ultragoal-runtime.ts#L996-L1062) |
| 8 | Tmux/worktree team runtime | pane, worker lifecycle, task evidence, worktree integration을 state로 모델링 | [team-runtime.ts#L49-L93](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/gjc-runtime/team-runtime.ts#L49-L93), [team/SKILL.md#L151-L173](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/defaults/gjc/skills/team/SKILL.md#L151-L173) |
| 9 | 역할별 model profile | default와 role agent별 model selector를 나눔 | [model-profiles.ts#L56-L120](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/config/model-profiles.ts#L56-L120), [architect.md#L1-L10](https://github.com/Yeachan-Heo/gajae-code/blob/0464f0f92a278ed34059db48d9951cbf41c1ce78/packages/coding-agent/src/prompts/agents/architect.md#L1-L10) |

## 자가 점검 및 미포함 영역

**전 주장 인용 자가 점검 완료.** 본문 주요 설계 주장은 고정 체크아웃(`0464f0f92a278ed34059db48d9951cbf41c1ce78`)의 permalink와 라인 범위에 연결했습니다.

이 문서가 다루지 않은 영역도 명시합니다. `packages/ai` provider client 내부, `packages/agent` generic runtime 내부, TUI rendering 세부, stats dashboard, robogjc Python service, Rust native crates, generated docs index, release/publish scripts, full test suite, 모든 CLI subcommand 구현, 모든 workflow skill body의 세부 문단, extension/discovery capability, browser/debug/eval 도구 내부 구현, bridge protocol 상세는 이 문서의 범위 밖입니다.

후속 분석에서 해당 주장이 필요할 때 별도 표본으로 인용하는 편이 안전합니다.

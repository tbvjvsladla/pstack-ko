# pstack

> [!NOTE]
> **한국어 학습·연구 에디션 (`pstack-ko`) 안내**
>
> 이 저장소는 원본 [`pstack`](https://github.com/cursor/plugins/tree/main/plugins/pstack)(by [poteto](https://x.com/poteto))의 핵심 자산 대부분이 정교한 영문 **마크다운(프롬프트·플레이북·엔지니어링 원칙·형식검증 스킬)**으로 구성되어 있는 점에 착안하여, **한국인 AI 개발자들이 `pstack`의 프롬프트 설계와 에이전트 하네스 아키텍처를 깊이 있게 공부(수학)하고 분석할 수 있도록 전체 마크다운(169개 문서)을 한국어로 번역해 배포하는 학습 목적의 프로젝트**입니다.

저는 [poteto](https://x.com/poteto)입니다. 사장이나 CEO는 아니지만 Meta, Netflix, Cursor에서 수백만 줄의 코드를 다뤄왔습니다. 또한 React 코어 팀의 일원으로서 React 컴파일러를 구축하고 유지보수하는 일을 돕고 있습니다.

AI가 저품질 슬롭(slop) 코드를 너무 많이 찍어낸다는 인식이 점점 커지고 있습니다. 저도 동의합니다. 스무 명의 슬롭 장인으로 꾸려진 팀처럼 배포하고 싶지는 않습니다. 품질 없는 처리량(throughput)은 제가 추구하는 목표가 아닙니다. 빠르게 가고 싶다면 먼저 깊이 들어가야 합니다.

**pstack은 그에 대한 저의 답입니다.** 여기에 담긴 스킬들은 제가 Cursor에서 고품질 코드를 배포하기 위해 매일 사용하는 것과 똑같습니다. 이 플러그인은 Cursor를 진짜 엔지니어링 팀으로 바꿔줍니다. 목표는 코드 줄 수(LOC)를 극대화하는 것이 아니며, 오히려 그 반대입니다. pstack은 더 적지만 더 높은 품질의 코드를 작성하도록 돕습니다.

**pstack은 두려움 없는 병렬 작업을 가능하게 합니다.** 에이전트 하나가 깊이 파고들어 검증 가능한 좋은 코드를 작성할 수 있다고 신뢰할 수 있을 때, 비로소 확신을 가지고 작업을 병렬화할 수 있습니다. `poteto-mode`로 여러 에이전트를 띄우고, 에이전트들이 작업에 엄격한 엔지니어링 원칙을 적용할 것이라고 믿고 맡기세요.

**Cursor는 모든 모델의 장점을 결합해 줍니다.** 모든 프론티어 모델에는 각자의 강점과 약점이 있습니다. pstack에서는 어떤 모델이든 사용할 수 있습니다. 실제로 제 스킬 중 상당수는 각 모델의 고유한 강점을 활용하기 위해 멀티 모델 워크플로우를 사용합니다.

포크하세요. 개선하세요. 여러분의 것으로 만드세요. PR은 언제나 환영합니다!

## 설치

```bash
/add-plugin pstack
```

Claude Code에서는 다음과 같이 설치합니다:

```bash
/plugin marketplace add cursor/plugins
/plugin install pstack@cursor-plugins
```

Claude Code는 서드파티 마켓플레이스에 대해 자동 업데이트를 기본적으로 꺼두므로, pstack은 처음 설치한 버전으로 유지됩니다. 자동 업데이트를 켜려면 `/plugin`을 실행하고 **marketplaces** 탭을 연 다음 `cursor-plugins`를 선택하고 **enable auto-update**를 고르세요. 수동으로 한 번만 업데이트하려면 `claude plugin update pstack@cursor-plugins`를 실행하세요.

Claude Code는 플러그인 명령어와 에이전트 앞에 접두사를 붙이므로 `/poteto-mode`는 `/pstack:poteto-mode`가 됩니다. [Claude Code 어댑터](./docs/harness/claude-code.md)가 나머지 모든 Cursor 이름을 매핑합니다. Claude Code에서 `/pstack:setup-pstack`은 모델 선택 결과를 `~/.claude/rules/pstack-models.md`에 기록하며, Bugbot이나 Automations처럼 Cursor 전용 기능이 필요한 단계는 한 줄짜리 안내와 함께 건너뜁니다.

Claude Code는 저장소 루트의 [`harness/pstack/`](https://github.com/cursor/plugins/tree/main/harness/pstack)에서 이 README와 가이드를 포함한 pstack의 생성 사본을 설치합니다. pstack을 수정한 뒤에는 저장소 루트에서 `node scripts/generate-pstack-harness-trees.mjs`를 실행하고 결과를 커밋하세요. 생성된 사본이 최신 상태가 아니면 CI가 실패합니다.

## 시작하기

두 단계면 됩니다:

1. [`/setup-pstack`](./skills/setup-pstack/SKILL.md)을 실행하여 추론 예산(reasoning budget)을 고르고 사용할 모델을 선택합니다.
2. 엄격함이 필요한 작업을 할 때마다 [`/poteto-mode`](./skills/poteto-mode/SKILL.md)를 사용합니다.

처음 오셨나요? [pstack 가이드](./docs/guide/README.md)를 따라가면 설정과 프롬프트 작성부터 검증 및 야간 자율 실행(overnight run)까지 첫 번째 실전 과업을 차근차근 경험할 수 있습니다. 막히거나 어떤 스킬이 맞는지 잘 모르겠다면 [`/poteto-help`](./skills/poteto-help/SKILL.md)에 물어보세요.

그게 전부입니다. 다른 스킬들은 상황에 따라 쓰이며, 모드 스킬이 필요할 때 알아서 호출해 줍니다. 기본 설정에서 모드는 모델의 강점에 따라 작업을 나눕니다: 코드 위임 작업(기능 구현, 리팩터링, 버그 수정, 성능 개선, 힐클라이밍)은 Grok으로 가고, 가장 까다로운 변경 사항, 산문 작성, 판단 작업은 Opus 5.5로 갑니다. 기본 패널은 Opus 5.5 / Grok입니다. [`/setup-pstack`](./skills/setup-pstack/SKILL.md)으로 이 설정을 언제든 바꿀 수 있습니다.

## 사용법

과업을 시작할 때 [`/poteto-mode`](./skills/poteto-mode/SKILL.md)를 사용하세요. 요청을 읽고 플레이북 목록에서 알맞은 것을 고른 뒤, 각 단계에 필요한 다른 스킬들을 실행합니다.

과업 내용을 입력하기 전에 `/` 메뉴에서 `/poteto-mode`를 먼저 선택하여 Cursor가 메시지에 해당 스킬을 첨부하도록 하세요.

### 그냥 [`/poteto-mode`](./skills/poteto-mode/SKILL.md)를 쓰세요

이 스킬이 핵심 단축키입니다. 저는 에이전트에게 엄격한 엔지니어링 작업을 시켜야 할 때마다 이 스킬을 씁니다. 여기에는 23개의 플레이북이 포함되어 있습니다:

```
/poteto-mode this pr has a subtle bug where the scroll drifts every 750ms even when idle. repro
first, then fix and verify.
```

```
/poteto-mode i'm going to bed. land the stack even if ci flakes. i want everything merged by
morning.
```

<details>
<summary>23개의 플레이북</summary>

| 플레이북 | 용도 |
|---|---|
| [investigation](./skills/poteto-mode/playbooks/investigation.md) | 읽기 전용 질문. x가 어떻게 작동하는지, 왜 y가 이렇게 만들어졌는지, 우리가 확실히 알고 있는지 조사합니다. |
| [bug fix](./skills/poteto-mode/playbooks/bug-fix.md) | 결함을 재현하고, 근본 원인을 찾아내며, 런타임 증거와 함께 수정합니다. |
| [perf](./skills/poteto-mode/playbooks/perf-issue.md) | 측정된 속도 저하를 추적하고 베이스라인 대비 성능을 개선합니다. |
| [hillclimb](./skills/poteto-mode/playbooks/hillclimb.md) | 목표치를 향해 단일 지표를 지속적이고 과학적으로 개선합니다. 전후 측정을 동반한 가설 루프를 돌며 채택된 개선마다 커밋 하나씩을 남깁니다. |
| [runtime forensics](./skills/poteto-mode/playbooks/runtime-forensics.md) | 계측(instrumentation)을 통해 라이브 증상(메모리 누수, 유휴 상태 CPU 스핀, 글리치)을 진단합니다. |
| [trace forensics](./skills/poteto-mode/playbooks/trace-forensics.md) | 캡처된 프로파일링 아티팩트(cpuprofile, trace, spindump, 힙 스냅샷)를 진단합니다. |
| [feature](./skills/poteto-mode/playbooks/feature.md) | 명명된 데이터 형태(data shape)로부터 출발해 새로운 동작이나 변경된 동작을 구축합니다. |
| [refactoring](./skills/poteto-mode/playbooks/refactoring.md) | 동작을 보존하면서 구조나 형태를 변경합니다. |
| [prototype](./skills/poteto-mode/playbooks/prototype.md) | 설계나 동작 결정을 저렴하게 내리거나 경험적 분기점을 직접 관찰하여 해결하기 위해 일회용 스케치를 만듭니다. |
| [visual parity](./skills/poteto-mode/playbooks/visual-parity.md) | 두 구현체 사이의 픽셀 단위 UI 동등성을 맞춥니다. |
| [authoring a skill](./skills/poteto-mode/playbooks/authoring-a-skill.md) | SKILL.md를 작성하거나 편집합니다. |
| [eval](./skills/poteto-mode/playbooks/eval.md) | 스킬이나 프롬프트 변경이 에이전트 동작에 미치는 영향을 블라인드 방식으로 테스트합니다. |
| [babysit](./skills/poteto-mode/playbooks/babysit.md) | 충돌 해결, 리뷰 스레드 대응, CI 통과까지 PR이나 스택을 머지 가능한 상태로 이끕니다. |
| [shipping](./skills/poteto-mode/playbooks/shipping.md) | 그린 상태인 스택을 독립적으로 검증한 뒤, 기본적으로는 GitHub을 통해(또는 사용 가능한 경우 origin을 통해) 연속된 검증 완료 구간을 아래에서 위로 랜딩합니다. |
| [autonomous run](./skills/poteto-mode/playbooks/autonomous-run.md) | 긴 과업을 멈추지 않고 완료될 때까지 몰고 갑니다. |
| [orchestrate](./skills/poteto-mode/playbooks/orchestrate.md) | 하나의 코디네이터 채팅에 맡겨진 상시 프로젝트: 며칠에 걸친 작업, 다수의 스택 PR, 서브에이전트 함대를 조율합니다. |
| [autopilot-full](./skills/poteto-mode/playbooks/autopilot-full.md) | PR당 한 명의 오너를 두고 독립적인 PR들을 머지까지 이끕니다. 스웜(swarm)이 코드 준비가 끝난 각 헤드를 검증하며, 이후 패치에 가해지는 모든 변경은 새로운 검사를 받습니다. |
| [autopilot-stack](./skills/poteto-mode/playbooks/autopilot-stack.md) | 머지만 제외한 autopilot-full입니다. 오퍼레이터가 리뷰하고 랜딩할 수 있도록 PR을 빌드 및 검증하며, 의존하는 PR 위에만 스택을 쌓습니다. |
| [session pickup](./skills/poteto-mode/playbooks/session-pickup.md) | 이전 에이전트가 진행 중이던 작업을 재개하거나 이어받습니다. |
| [pause safely](./skills/poteto-mode/playbooks/pause-safely.md) | 진행 중인 작업을 나중에 깔끔하게 재개할 수 있도록 안전하게 일시 중지합니다. |
| [multi-phase plan](./skills/poteto-mode/playbooks/multi-phase-plan.md) | 여러 단계(phase)나 스택 PR에 걸쳐 진행되는 작업을 다룹니다. |
| [worktree cleanup](./skills/poteto-mode/playbooks/worktree-cleanup.md) | 안전 게이트를 거쳐 머지되었거나 버려진 워크트리(worktree)와 오래된 iOS 시뮬레이터를 정리하여 디스크 공간을 확보합니다. |
| [opening a pr](./skills/poteto-mode/playbooks/opening-a-pr.md) | 정돈된 작은 커밋들로부터 Conventional Commits 제목과 브리핑 스타일의 본문을 갖춘 준비된 PR을 엽니다. 다른 모든 플레이북의 마지막에 호출됩니다. |

</details>



호출되면 다음 작업을 수행합니다:

1. 과업을 [플레이북](./skills/poteto-mode/playbooks/)에 매칭하고, 플레이북의 단계들을 그대로 복사해 첫 항목들로 채운 할 일 목록(todo list)을 엽니다.
2. 각 단계가 실행될 때 필요한 다른 스킬들로 라우팅합니다.
3. 소비자와 메인테이너의 관점에 맞춰 군더더기 없는(unslopped) 응답을 작성합니다.

전체 규칙과 플레이북은 [`skills/poteto-mode/SKILL.md`](./skills/poteto-mode/SKILL.md)에 있습니다.

여러 턴에 걸쳐 [`/poteto-mode`](./skills/poteto-mode/SKILL.md)를 켜두려면 `/` 메뉴에서 선택한 뒤 Enter 대신 Option+Enter(Mac) 또는 Alt+Enter(Windows)를 누르세요. 그러면 Cursor가 에이전트 창과 CLI에서 제공하는 [커스텀 모드(custom mode)](https://cursor.com/docs/skills#using-a-skill-as-a-custom-mode)로 전환됩니다. 모드를 종료할 때까지 매 턴마다 컨텍스트에 유지됩니다. 그냥 Enter를 누르면 해당 메시지 하나에만 첨부됩니다.

[`/poteto-mode`](./skills/poteto-mode/SKILL.md)는 Cursor의 `/loop` 명령어와도 궁합이 매우 좋습니다. 엄격함을 희생하지 않고도 Cursor가 여러 시간 동안 작업하게 만들 수 있습니다.

## 스킬

[`/poteto-mode`](./skills/poteto-mode/SKILL.md)는 단계에서 필요할 때 대부분의 스킬(`how`, `why`, `architect`, `arena`, `swarm`, `interrogate`, `unslop`, `no-comments`, `technical-writing`, `tdd`, 그리고 원칙 스킬들)을 대신 실행해 줍니다. 아래 표는 개별 스킬을 직접 호출하고 싶을 때 참고하세요:

```
/how do we cancel runs? do we have an n+1 when we look up every run to cancel?
```

```
/interrogate review this pr.
```

<details>
<summary>모든 스킬</summary>

| 스킬 | 사용 시점 |
|---|---|
| [`/poteto-mode`](./skills/poteto-mode/SKILL.md) | 자명하지 않은 모든 과업의 기본 진입점입니다. |
| [`/poteto-help`](./skills/poteto-help/SKILL.md) | pstack이 처음이거나 어떤 스킬, 플레이북, 원칙이 맞는지 확실하지 않을 때 씁니다. 무엇을 하려는지 파악해 해당 부분을 답해주고 입력할 프롬프트를 건네줍니다. `/poteto-help`를 직접 입력할 때만 실행됩니다. |
| [`/in-your-own-words`](./skills/in-your-own-words/SKILL.md) | 에이전트가 작업을 시작하기 전에 목표와 문제를 자신의 말로 되짚어보게 하고 싶을 때 씁니다. `/in-your-own-words`를 직접 입력할 때만 실행됩니다. |
| [`/sharpen`](./skills/sharpen/SKILL.md) | 프롬프트를 보내기 전에 길이를 늘리는 대신 깊이를 더하고 싶을 때 씁니다: 최고 수준의 전문가가 요청받지 않았더라도 챙겼을 사항, 출발점으로 삼을 원재료, 방법론과 도구, 그리고 결과가 맞는지 확인하는 외부 검증 기준을 짚어줍니다. 다시 쓴 프롬프트를 돌려주며 선택권은 사용자에게 남겨둡니다. 과업 자체를 수행하지는 않으며, `/sharpen`을 직접 입력할 때만 실행됩니다. |
| [`/how`](./skills/how/SKILL.md) | 서브시스템이 어떻게 작동하는지 상세한 워크스루를 원할 때 씁니다. |
| [`/why`](./skills/why/SKILL.md) | 왜 무언가가 지금처럼 만들어졌는지 알고 싶을 때 씁니다. 런타임에 사용 가능한 MCP를 탐색하고 각 증거 범주(소스 컨트롤, 이슈 트래커, 장문 문서, 실시간 채팅, 인프라 관측성, 에러 트래킹, 애널리틱스 웨어하우스)를 병렬로 질의합니다. |
| [`/recall`](./skills/recall/SKILL.md) | 작업을 시작하거나 재개할 때 본인의 채팅 기록과 공유 기록으로부터 특정 주제의 최근 맥락을 재구성하여 간결한 현재 상태 브리핑으로 돌려받고 싶을 때 씁니다. |
| [`/ketchup`](./skills/ketchup/SKILL.md) | 마지막 메시지 이후 이 채팅에서 무엇이 바뀌었는지 평이한 말과 작은 시각 자료로 확인하고, 사용자의 결정을 기다리는 모든 항목을 나열해 응답만 보고도 바로 결정할 수 있게(응답이 길어지더라도) 받고 싶을 때 씁니다. |
| [`/ketchup-video`](./skills/ketchup-video/SKILL.md) | `/ketchup` 요약을 짧은 자막 슬라이드 비디오로 보고, 결정을 기다리는 사안들은 그 아래에 텍스트로 받고 싶을 때 씁니다. |
| [`/blast-radius`](./skills/blast-radius/SKILL.md) | 작아 보이는 변경이 다른 어디를 깨뜨릴 수 있는지 파악하고, 안전하다는 핵심 근거 하나를 단순 주장이 아니라 실제 코드 실행으로 증명받고 싶을 때 씁니다. |
| [`/architect`](./skills/architect/SKILL.md) | 함수 경계를 넘나드는 코드를 작성하기 직전에 호출자 사용 방식, 타입, 모듈 형태를 먼저 확정하고 싶을 때 씁니다. |
| [`/arena`](./skills/arena/SKILL.md) | 동일한 작업에 대해 N개의 병렬 시도를 실행한 뒤 각각의 장점만 취합하고 싶을 때 씁니다. |
| [`/swarm`](./skills/swarm/SKILL.md) | 서로 다른 슬라이스나 경합 구간에 N개의 병렬 워커를 투입한 뒤 하나의 종합 보고서를 받고 싶을 때 씁니다. |
| [`/interrogate`](./skills/interrogate/SKILL.md) | diff를 두고 여러 모델이 이를 깨뜨려 보게 한 다음, 실제 실행으로 입증된 발견 사항만 남기고 싶을 때 씁니다. |
| [`/automate-me`](./skills/automate-me/SKILL.md) | 실제 작업 기록을 바탕으로 나만의 `-mode` 스킬 초안을 만들고 싶을 때 씁니다. |
| [`/make-bot-ui`](./skills/make-bot-ui/SKILL.md) | 발신자 키 핸드오프와 Tailscale을 포함해 웹훅으로 Grok Bot을 깨우는 버튼이 달린 페이지나 대시보드를 만들고 싶을 때 씁니다. |
| [`/setup-pstack`](./skills/setup-pstack/SKILL.md) | 역할별로 pstack이 사용할 모델을 고르고 싶을 때 씁니다. 사용 가능한 모델을 감지해 설정 규칙 파일을 작성합니다. |
| [`/reflect`](./skills/reflect/SKILL.md) | 긴 과업을 마친 뒤 거기서 얻은 레시피를 스킬 수정 사항으로 갈무리하고 싶을 때 씁니다. |
| [`/correct`](./skills/correct/SKILL.md) | 에이전트의 똑같은 실수를 반복해서 교정하고 있을 때 씁니다. 기록에서 실수 유형을 마이닝하고, 작동하는 가장 높은 수준(아키텍처 → 타입, 린트, CI → 테스트 → 마지막으로 문서)에서 각 실수를 수정하며, 각 규칙과 그 강제 수단을 짝지은 표를 유지합니다. |
| [`/teach`](./skills/teach/SKILL.md) | 변경 사항이나 서브시스템을 단순 요약이 아니라 진짜로 이해하고 싶을 때 씁니다. `how` + `why`를 실행하고 다이어그램을 하나씩 쌓아 올리며 평이한 설명 하나로 엮어냅니다. |
| [`/tdd`](./skills/tdd/SKILL.md) | 버그를 수정할 때 저렴한 로컬 테스트 경로가 있는 경우 씁니다. 실패하는 테스트를 먼저 작성한 뒤 수정을 진행합니다. |
| [`/benchmark-checklist`](./skills/benchmark-checklist/SKILL.md) | 벤치마크를 돌렸거나 속도 향상 또는 회귀를 측정했을 때 씁니다. 수치를 보고하거나 그에 따라 행동하기 전에 그 수치(병목 요인, 튜닝, 에러, 반복 실행, 엔드투엔드 유의미성)를 검증합니다. |
| [`/no-comments`](./skills/no-comments/SKILL.md) | 리뷰 전에 주석을 걷어냅니다. Comment Sicko를 스폰하고, 수용된 지적 사항을 수정하며, 주석이 주장하던 제약 조건을 코드로 인코딩할 방법을 제안합니다. |
| [`/typescript-best-practices`](./skills/typescript-best-practices/SKILL.md) | TypeScript를 읽거나 편집할 때 씁니다. `type-system-discipline` 원칙을 구체적인 문법으로 풀어냅니다. |
| [`/figure-it-out`](./skills/figure-it-out/SKILL.md) | 번들로 제공되는 플레이북 중 맞는 것이 없을 때 씁니다. 과업에 맞는 엄격하고 감사 가능한(auditable) 플레이북을 설계합니다. |
| [`/show-me-your-work`](./skills/show-me-your-work/SKILL.md) | 리뷰 가능한 의사결정 기록을 남기고 싶을 때 씁니다. 커밋할 수 있는 TSV 파일에 의사결정을 기록합니다. |
| [`/create-verification-skill`](./skills/create-verification-skill/SKILL.md) | 프로젝트에 앱 동작을 증명할 스크립트 수단이 없을 때 씁니다. 언어나 플랫폼에 구애받지 않고 기능 맵(feature map)을 갖춘 프로젝트 로컬 검증 스킬을 생성합니다. |
| [`/maintain-verification-skill`](./skills/maintain-verification-skill/SKILL.md) | 검증 스킬의 기능 맵이 실제 앱과 어긋났을 때 씁니다. 소스 웨이브 + 1회의 라이브 패스를 거쳐 입증된 수정 사항을 최대 하나의 PR로 묶어냅니다. |
| [`/create-formal-verification-skill`](./skills/create-formal-verification-skill/SKILL.md) | 경쟁 상태(race), 재시도, 상태 머신, 파서, 안전하지 않은 코드를 깨뜨리는 케이스에 일반 테스트가 도달할 수 없을 때 씁니다. 적합한 도구로 속성 하나를 검증하고 변경 시마다 재검사하는 프로젝트 로컬 스킬을 생성합니다. |
| [`/maintain-formal-verification-skill`](./skills/maintain-formal-verification-skill/SKILL.md) | 정형 검증이 다루는 코드가 마지막 검사 이후 변경되었을 때 씁니다. 모델, 하네스 또는 속성을 코드에 맞게 업데이트하고 재실행한 뒤, 각 실패가 검증의 드리프트인지 실제 코드의 버그인지 판별합니다. |
| [`/statechart`](./skills/statechart/SKILL.md) | 동작의 오너가 명세서를 읽는 대신 눈으로 직접 확인해야 할 때 씁니다. TLA+ 모델을 검사하고, 사용자에게 보이는 모습을 Mermaid 상태차트(statechart)로 그리며, 규칙을 위반하는 각 상태를 표시하고 거기까지 가는 최단 경로를 제시합니다. |
| [`/unslop`](./skills/unslop/SKILL.md) | 글을 다듬을 때 씁니다. AI 특유의 상투적 표현(AI tells)을 제거합니다. |
| [`/bro`](./skills/bro/SKILL.md) | 방금 메시지를 전문 용어 없이 평이한 일상 언어로 다시 설명받고 싶을 때 씁니다. |
| [`/technical-writing`](./skills/technical-writing/SKILL.md) | 문서, RFC, README, PR 설명, 커밋 메시지를 위한 계층형 문서 표준(Diátaxis + Google 개발자 스타일 + STE + Global English)입니다. |

</details>



### 예시

대부분의 경우 저는 과업을 시작할 때 [`/poteto-mode`](./skills/poteto-mode/SKILL.md)를 입력하고 플레이북으로 라우팅하게 둡니다. 다른 스킬들은 단계에서 필요할 때 자동으로 실행됩니다. 그중 몇 가지는 제가 직접 호출해서 씁니다.


<details>
<summary>모든 예시</summary>

```
bug fix:           /poteto-mode this pr has a subtle bug where the scroll drifts every 750ms even
                   when idle. repro first, then fix and verify.
perf:              /poteto-mode a big list takes a second or two to load even though we virtualize.
                   run a cpu trace and tell me why.
feature:           /poteto-mode build a small feature behind a feature flag. verify it really works.
prototype:         /poteto-mode build two prototypes of the markdown renderer so we can compare.
                   spawn an agent for each.
multi-phase:       /poteto-mode open source these skills as a plugin. nothing internal leaks, work
                   in a temp dir, show me the dependency graph first.
overnight run:     /poteto-mode i'm going to bed. land the stack even if ci flakes. i want
                   everything merged by morning.
babysit:           /poteto-mode check on pr 123. anything outstanding?
visual parity:     /poteto-mode the row spacing is too tall when this flag is on. the second image
                   is correct. repro and fix until it matches.
figure it out:     /poteto-mode i'm stepping away. migrate every caller from the synchronous store
                   to the new async one, keeping behavior identical. i want to trust it was done
                   right when i'm back.
how:               /how do we cancel runs? do we have an n+1 when we look up every run to cancel?
why:               /why is this feature flag not on yet?
architect:         design this instrumentation to be high signal with no false positives. /architect
                   this first.
arena:             /arena take my prompt to the arena verbatim. i want to compare their proposals
                   with yours.
swarm:             /swarm check every package under packages/ against its check.sh. one worker per
                   package. one report.
interrogate:       /interrogate review this pr.
tdd:               /tdd implement
unslop:            can we unslop and tighten the new changes?
reflect:           /reflect that took too long. capture what we learned so the next run doesn't
                   repeat it.
correct:           /correct
show-me-your-work: /show-me-your-work keep a decision trail i can review when i'm back.
automate-me:       /automate-me
help:              /poteto-help which skill should i use to review this branch?
```

</details>

## `poteto-agent` 및 Comment Sicko 서브에이전트

[`/poteto-mode`](./skills/poteto-mode/SKILL.md)는 이를 호출받은 채팅에서 실행됩니다. 그 채팅이 플레이북, 할 일 목록, 그리고 모든 서브에이전트 스폰을 소유합니다.

또한 pstack은 실행 도중 스폰되는 위임 에이전트용 래퍼로 [`subagent_type: "poteto-agent"`](./agents/poteto-agent.md)를 함께 제공합니다. 이 서브에이전트는 작업을 시작하기 전에 인라인 원칙 색인을 포함한 `poteto-mode` 전체를 읽습니다. 이를 `generalPurpose`로 대체하면 그 읽기 과정을 건너뛰어 의도에서 벗어나게 됩니다. 플레이북 자체를 실행하기 위해 `poteto-agent`를 스폰하지는 마세요.

[`/poteto-mode`](./skills/poteto-mode/SKILL.md)와 [`subagent_type: "poteto-agent"`](./agents/poteto-agent.md)는 동일한 래퍼를 통해 라우팅됩니다.

또한 pstack은 `subagent_type: "comment-sicko"`로 사용할 수 있는 읽기 전용 주석 리뷰어인 [Comment Sicko](./agents/comment-sicko.md)를 제공합니다. 보통 직접 호출하기보다 [`/no-comments`](./skills/no-comments/SKILL.md)를 통해 호출합니다.

## 원칙 (Principles)

하나의 원칙마다 짧은 스킬 하나씩, 총 24개의 스킬로 구성됩니다. `poteto-mode`는 이를 인라인으로 색인하고 과업 시작 시 그 색인을 읽습니다. 독립된 파일들은 다른 스킬이 이름으로 원칙을 참조할 수 있게 하고, 색인이 각 원칙의 전체 규칙을 가리킬 수 있도록 존재합니다.

<details>
<summary>24개의 모든 원칙</summary>

| 원칙 | 그룹 | 규칙 |
|---|---|---|
| [laziness-protocol](./skills/principle-laziness-protocol/SKILL.md) | core | 삭제를 우선하고 문제를 해결하는 가장 작은 변경을 지향합니다. |
| [foundational-thinking](./skills/principle-foundational-thinking/SKILL.md) | core | 로직을 작성하기 전에 적용합니다: 핵심 타입과 자료구조를 고르고, 스캐폴드 대 기능 작업의 순서를 정하며, 동시성 액터들이 무엇을 공유하는지 묻습니다. 자료구조를 제대로 잡아 다운스트림 코드가 자명해지게 만듭니다. |
| [redesign-from-first-principles](./skills/principle-redesign-from-first-principles/SKILL.md) | core | 요구사항을 나중에 덧붙이는 대신, 첫날부터 기초 가정이었다면 어떻게 설계했을지 생각하며 재설계합니다. |
| [attack-the-premise](./skills/principle-attack-the-premise/SKILL.md) | core | 하나의 전제를 공유하는 두 개 이상의 수정안이 동일한 게이트를 통과하지 못했을 때 적용합니다. 그 전제를 가정하는 또 다른 수정안을 작성하기 전에 전제를 명시적으로 적고 직접 테스트합니다. |
| [subtract-before-you-add](./skills/principle-subtract-before-you-add/SKILL.md) | core | 불필요한 군더더기, 중복 검증기, 스텁 참조를 먼저 제거한 다음 더 단순해진 기반 위에 구축합니다. |
| [minimize-reader-load](./skills/principle-minimize-reader-load/SKILL.md) | core | 질문과 답 사이의 계층 수, 그리고 읽는 이의 머릿속에 담아야 할 숨겨진 상태를 줄입니다. 호출자가 하나뿐인 래퍼를 허물고 가변(mutable) 범위를 좁힙니다. |
| [outcome-oriented-execution](./skills/principle-outcome-oriented-execution/SKILL.md) | core | 명시적인 단계 경계가 있는 계획된 재작성 및 마이그레이션 중에 적용합니다. 일회용 호환성 코드로 매끄러운 중간 상태를 유지하려 하지 말고 목표 아키텍처로 수렴시킵니다. |
| [experience-first](./skills/principle-experience-first/SKILL.md) | core | 구현 편의성보다 사용자 만족을 선택합니다. 거친 기능 여러 개보다 다듬어진 기능 소수를 배포합니다. |
| [exhaust-the-design-space](./skills/principle-exhaust-the-design-space/SKILL.md) | core | 확정하기 전에 경쟁하는 프로토타입 2~3개를 만들어 나란히 비교합니다. |
| [build-the-lever](./skills/principle-build-the-lever/SKILL.md) | core | 단순 대량 작업뿐만 아니라 모든 비자명한 작업(편집, 마이그레이션, 분석, 검사)에 적용합니다. 수작업으로 처리하는 대신 그 작업을 수행하거나 증명하는 도구(코드모드, 스크립트, 생성기 또는 서브에이전트가 따를 스킬)를 만듭니다. 그 도구가 리뷰어가 다시 실행해 볼 수 있는 아티팩트가 됩니다. |
| [model-the-domain](./skills/principle-model-the-domain/SKILL.md) | architecture | 흩어진 조건문 대신 구조체로 도메인을 인코딩합니다. |
| [boundary-discipline](./skills/principle-boundary-discipline/SKILL.md) | architecture | 시스템 경계(CLI, 설정, 네트워크, 외부 API)에 가드 로직을 집중시키고, 내부 타입은 신뢰하며 비즈니스 로직은 순수 함수로 유지합니다. |
| [type-system-discipline](./skills/principle-type-system-discipline/SKILL.md) | architecture | 잘못된 상태를 표현 불가능하게 만들고, 의미론적 원시 타입에 브랜드(brand)를 부여하며, 외부 데이터는 경계에서 파싱하고, 배리언트(variant)를 빠짐없이 처리하며, 권위 있는 스키마로부터 타입을 파생시킵니다. |
| [make-operations-idempotent](./skills/principle-make-operations-idempotent/SKILL.md) | architecture | 이전 실행이 부분적으로만 끝났더라도 항상 동일한 최종 상태로 수렴하게 만듭니다. |
| [migrate-callers-then-delete-legacy-apis](./skills/principle-migrate-callers-then-delete-legacy-apis/SKILL.md) | architecture | 호환성 계층을 유지하는 대신 같은 웨이브에서 호출자를 마이그레이션하고 구형 API를 삭제합니다. |
| [separate-before-serializing-shared-state](./skills/principle-separate-before-serializing-shared-state/SKILL.md) | architecture | 상태 공유 자체를 먼저 제거하고, 단일 공유 작성자(writer)가 진짜 불변식인 경우에만 구조적으로 직렬화합니다. |
| [prove-it-works](./skills/principle-prove-it-works/SKILL.md) | verification | 과업을 마친 뒤 완료를 선언하기 전에 적용합니다. 대리 지표, 자체 보고, 또는 '컴파일됩니다'가 아니라 실제 아티팩트(기능 실행, 실제 값 읽기, diff 검사)를 대상으로 검증합니다. |
| [fix-root-causes](./skills/principle-fix-root-causes/SKILL.md) | verification | 각 증상을 시스템이 처음 잘못된 지점까지 역추적하여 그곳에서 수정합니다. 가드(guard)는 그것이 처리하는 상태가 정상적인(legitimate) 상태일 때만 올바른 수정입니다. |
| [sequence-verifiable-units](./skills/principle-sequence-verifiable-units/SKILL.md) | verification | 다단계 작업(스윕, 마이그레이션, 유사한 편집의 연속 실행)과 커밋 및 PR을 쌓는 방식에 적용합니다. 작업을 각각 검증 가능한 상태로 끝나는 작은 단위로 쪼개고, 다음으로 넘어가기 전에 각각을 확인하며, 그 순서 자체가 리뷰어에게 스스로를 증명하도록 전달 순서를 배치합니다. |
| [test-behavior-not-implementation](./skills/principle-test-behavior-not-implementation/SKILL.md) | verification | 테스트를 작성, 변경 또는 유지할 때 적용합니다. 코드의 사용자가 관찰할 수 있는 것을 단언(assert)하여, 해당 동작이 깨질 때만 테스트가 실패하고 내부 구현만 바뀔 때는 계속 통과하게 만듭니다. |
| [explain-the-number](./skills/principle-explain-the-number/SKILL.md) | verification | 측정한 수치(속도 향상, 회귀, 처리량, 지연 시간, 평가 결과)를 신뢰하거나 보고하거나 그에 따라 행동하기 전에 적용합니다. 무엇이 그 수치를 제한하는지 찾아내고, 생각한 작업이 아닌 다른 것을 측정했을 가능성을 배제합니다. |
| [guard-the-context-window](./skills/principle-guard-the-context-window/SKILL.md) | delegation | 대량 데이터는 서브에이전트로 라우팅하고, 메인 스레드에는 원시 페이로드가 아니라 요약본만 유지합니다. |
| [never-block-on-the-human](./skills/principle-never-block-on-the-human/SKILL.md) | delegation | 먼저 진행하고 결과를 제시한 뒤 사람이 사후에 방향을 교정하게 합니다. 확인 요청은 되돌릴 수 없는(irreversible) 작업에만 아껴 둡니다. |
| [encode-lessons-in-structure](./skills/principle-encode-lessons-in-structure/SKILL.md) | meta | 텍스트를 더 덧붙이는 대신 린트, 메타데이터 플래그, 런타임 체크 또는 스크립트로 규칙을 인코딩합니다. |

</details>

## 여기에 포함되지 않는 항목

`poteto-mode`가 참조하지만 이 플러그인에 번들로 묶이지 않은 몇 가지 항목이 있습니다:

- `/deslop` 및 `deslop` 스킬은 `cursor-team-kit` 플러그인에 포함되어 있습니다.
- `control-cli`(CLI 및 TUI용)와 `control-ui`(브라우저, Electron, 웹용)도 `cursor-team-kit`에 포함되어 있습니다.
- `/create-skill`은 Cursor 내장 기능입니다. 또한 Cursor는 PR을 감시하는 내장 `/autopilot`을 제공합니다. `poteto-mode` 안에서는 PR 상태 확인 요청이 내장 기능 대신 [babysit 플레이북](./skills/poteto-mode/playbooks/babysit.md)으로 연결됩니다.

전체 세트를 원한다면 pstack과 함께 `cursor-team-kit`을 설치하세요.

## 왜 계획(planning) 스킬은 없나요?

Cursor에는 이미 pstack과 아주 잘 어우러지는 훌륭한 플랜 모드가 있습니다. 하지만 개인적으로 저는 계획 세우기를 믿지 않습니다. 최고의 명세서는 코드입니다. 굳이 계획을 세우고 싶다면 [`/poteto-mode`](./skills/poteto-mode/SKILL.md)가 이를 다루긴 하지만 기본값은 아닙니다.

## 나만의 것으로 만들기

`poteto-mode`는 저의 작업 스타일입니다. 여러분이 원하는 방식과 정확히 같지 않을 수 있습니다.

[`/automate-me`](./skills/automate-me/SKILL.md)를 입력해 보세요. 최근 트랜스크립트를 마이닝하여 여러분이 실제로 일한 방식으로부터 `<your-name>-mode` 스킬 초안을 작성하고, 내부적으로는 pstack을 통해 라우팅합니다. pstack을 기반으로 유지하면서 `poteto-mode`와 나란히 쓸 수 있는 자신만의 라우팅 스킬을 얻게 됩니다.

모델 역시 설정 가능합니다. [`/setup-pstack`](./skills/setup-pstack/SKILL.md)을 입력해 보세요. 접근 가능한 모델을 감지하고 각 역할(코드, 판단, 리뷰 패널)을 모델에 매핑하는 작은 상시 적용(always-applied) 규칙을 작성합니다. 모든 스킬이 이 규칙을 읽으며 규칙이 없을 때는 합리적인 기본값으로 폴백하므로, 바꾸고 싶은 부분만 오버라이드하면 됩니다.

기본값이 변경되더라도 변경 전에 작성된 규칙 파일은 여전히 예전 기본값을 고정(pin)합니다. 해당 역할 줄을 삭제하거나 파일을 삭제한 뒤 `/setup-pstack`을 다시 실행하세요. 다시 실행할 때는 기본값과 다른 모델이 지정된 역할은 그대로 유지됩니다.

## 자동화 (Automations)

또한 pstack은 비활성 상태의 [benny 자동화 팩](./automations/benny/)을 함께 제공합니다. benny는 Slack 이슈 제보를 트리아지한 뒤, 실제 UI 증거를 바탕으로 확인된 버그를 재현하고 수정합니다. 이 파일들은 슬래시 스킬로 등록되지 않습니다.

설정하려면 Cursor에 [`FOR_AGENTS.md`](./automations/benny/FOR_AGENTS.md)를 가리키세요. 셋업 과정은 대상 저장소의 `.cursor/automations/benny/`에 팩을 복사하고, 공유 스킬을 쓸 수 있도록 그곳에서 pstack을 활성화하며, 사용자 설정은 복사된 팩 바깥에 유지합니다. benny는 Cursor Automations 위에서 실행되므로 Cursor 전용입니다.

## 라이선스

MIT

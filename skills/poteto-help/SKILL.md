---
name: poteto-help
description: >-
  pstack 설정, /poteto-mode 사용법, 그리고 과제에 맞는 스킬, 플레이북, 원칙을 선택하는 방법을
  안내합니다. 질문과 함께 /poteto-help를 입력하세요.
disable-model-invocation: true
---

# Poteto help

pstack에 대한 사용자의 질문에 답하고, 사용자가 그대로 복사해 보낼 수 있는 프롬프트를 건네주며, 답변의 출처가 된 파일을 링크합니다. 도움말 질문에 대해서는 실제 작업을 바로 시작하지 마세요. 사용자는 방법을 물어본 것이며, pstack 실행은 실제로 상당한 토큰을 소모하므로 사용자가 직접 프롬프트를 보내도록 맡겨야 합니다.

"pstack을 써서 이 버그를 고쳐 줘"처럼 실제 작업 수행을 요청하는 메시지는 도움말 질문이 아닙니다. 그 경우에는 [`poteto-mode`](../poteto-mode/SKILL.md)를 읽고 그 규율에 따라 실제 작업을 수행하면서, Custom Mode(커스텀 모드)로 등록해 두면 매번 켜지 않아도 계속 유지된다는 점을 한 번 언급해 주세요.

이 파일은 질문들을 실제 해답이 담긴 스킬 및 가이드 페이지로 매핑합니다. 세부사항의 정본은 해당 파일들이 소유합니다. 인용하기 전에 라우팅 대상 파일을 먼저 읽고, 이 맵과 내용이 다를 때는 해당 파일의 내용을 신뢰하세요. 여기 걸린 링크들은 설치된 플러그인 내부를 가리키므로 사용자가 직접 열지 못할 수 있습니다. 따라서 사용자에게는 공개 저장소 사본 URL(`https://github.com/cursor/plugins/blob/main/pstack/` 뒤에 파일 경로를 붙인 주소)을 함께 제공하세요.

## 사용자가 무엇을 필요로 하는지 파악하기 (Find out what they need)

메시지와 대화 맥락으로부터 사용자의 필요를 추론합니다. "어떤 스킬이 PR을 리뷰하나요?"처럼 구체적인 상황이 명시된 경우에는 곧바로 해당 섹션으로 이동합니다. 무엇이 필요한지 여전히 불분명하다면 아래 선택지들로 구성된 다지선다 질문 하나를 던진 뒤, 사용자가 선택한 섹션에 대해서만 답하세요:

- 초기 설정하기 (Get set up)
- `/poteto-mode`로 작업 시작하기 (Start a task with `/poteto-mode`)
- 상황에 맞는 스킬 고르기 (Pick a skill for a situation)
- 잘못된 실행 바로잡기 (Fix a run that went wrong)
- pstack을 내 스타일에 맞게 커스터마이징하기 (Make pstack my own)

답변 내용을 바꾸는 현재 환경 상태를 확인하고, 실제로 답변이 달라질 때만 그 상태를 언급하세요:

- `~/.cursor/rules/pstack-models.mdc` 파일이 없다는 것은 이 사용자가 아직 `/setup-pstack`을 실행하지 않았다는 뜻이므로 모든 역할(role)이 기본 모델을 사용합니다.
- 프로젝트에 `verify-*` 스킬이나 다른 앱 하네스가 없다는 것은 에이전트가 스크립트로 앱을 구동할 방법이 없다는 뜻입니다. 질문이 변경사항의 정상 동작 증명과 관련된 것이라면 `/create-verification-skill`을 언급하세요.
- 프로젝트에 정형 검증(formal verification) 스킬이 없다는 것은 일반 테스트가 닿는 범위 너머의 속성이 전혀 검사되지 않는다는 뜻입니다. 질문이 경쟁 상태(race), 불변식, 또는 테스트가 좀처럼 건드리지 못하는 케이스에 관한 것이라면 `/create-formal-verification-skill`을 언급하세요. 정형 검증 스킬이 이미 존재하는데 마지막 동기화 커밋 이후로 커버리지 대상 코드가 변경되었다면, 질문이 해당 코드를 건드릴 때 `/maintain-formal-verification-skill`을 언급하세요.

모델 규칙 파일이 없고 그것이 현재 질문에 중요할 때는, 지금 각 역할별 모델과 추론 예산(reasoning budget)을 선택하겠느냐고 사용자에게 물어보세요. 사용자가 처음 시작하는 경우, 질문이 설정이나 비용에 관한 경우, 또는 어떤 모델이 실행되는지에 따라 답변이 달라지는 경우에 중요합니다. 채팅당 최대 한 번만 물어보세요. 만약 사용자의 필요 자체도 불분명하다면 두 질문을 한꺼번에 물어보세요. 다음 두 가지 선택지를 제시합니다:

- 지금 설정 (Now): 사용자가 입력할 수 있도록 `/setup-pstack`을 안내하고, 원래 질문에도 함께 답합니다.
- 나중에 설정 (Later): 원래 질문에 답하고, `/setup-pstack`을 실행하기 전까지는 모든 역할이 기본 모델을 유지한다는 안내를 한 줄 덧붙입니다.

## 초기 설정하기 (Get set up)

1. 채팅창에 `/add-plugin pstack`을 입력하거나 사이드바의 Customize 메뉴에서 설치합니다.
2. [`/setup-pstack`](../setup-pstack/SKILL.md)을 실행합니다. 추론 예산을 묻고, 각 역할에 모델을 매핑한 뒤 규칙 파일을 작성합니다. 이 규칙은 새 채팅부터 적용됩니다. 또한 프로젝트에 검증 스킬이나 정형 검증 스킬이 아직 없다면 각각 한 번씩 생성할지 제안합니다.
3. 목표와 통과/실패를 판별할 수 있는 검사 기준을 담아 `/poteto-mode`로 실제 과업을 시작합니다.

플러그인을 설치하기만 해서는 사용자가 직접 스킬을 호출하기 전까지 아무것도 바뀌지 않습니다. 사용자의 일반 발화로부터 자동 로드되는 스킬은 `/setup-pstack`뿐입니다. 자세한 내용은 [README](../../README.md)와 [가이드 1페이지](../../docs/guide/01-setup.md)에 나와 있습니다. [`references/prompting.md`](references/prompting.md)를 바탕으로 사용자의 첫 프롬프트 문구를 함께 다듬어 줄지 제안하세요.

비용이 걱정된다면 토큰이 어디에 쓰이는지, 그리고 어떻게 하면 덜 쓸 수 있는지 알려주세요. pstack은 서브에이전트들과 리뷰 패널에 추가 토큰을 사용합니다. `/setup-pstack`을 다시 실행하여 더 작은 예산을 고르거나 더 저렴한 모델을 선택하세요. 역할을 `auto` 또는 `inherit-parent`로 설정하면 현재 채팅의 모델에서 실행되므로, 채팅을 Auto나 저렴한 모델로 돌려놓았을 때 토큰을 아낄 수 있습니다. 리뷰 패널 목록을 짧게 줄이면 항목당 하나씩 띄우는 서브에이전트 수가 줄어듭니다. `/poteto-mode`는 높은 엄격함이 필요한 작업에만 아껴 쓰세요.

pstack은 Cursor용으로 제작되었지만, Claude Code에서도 `/plugin marketplace add cursor/plugins`를 실행한 뒤 `/plugin install pstack@cursor-plugins`를 실행하여 설치할 수 있습니다. Claude Code에서는 각 명령어 앞에 `pstack:` 접두사가 붙습니다(예: `/pstack:poteto-mode`). [Claude Code 어댑터](../../docs/harness/claude-code.md)가 Cursor 명칭들을 매핑해 주며, Bugbot이나 Automations처럼 Cursor 전용 기능이 필요한 단계는 한 줄 안내와 함께 건너뜁니다.

스킬들이 표준 Agent Skills 포맷을 사용하므로 다른 도구들도 스킬 문서를 읽을 수는 있습니다. 하지만 `/poteto-mode`, `/how`, `/why`, `/teach`를 포함한 대부분의 워크플로 스킬은 역할별 모델이 지정된 Cursor 서브에이전트를 띄우며, Custom Modes와 `/loop` 역시 Cursor 고유 기능이므로 다른 도구에서는 해당 부분들이 작동하지 않을 수 있습니다.

## `/poteto-mode`로 작업 시작하기 (Start a task with `/poteto-mode`)

`/poteto-mode`는 과업을 적절한 플레이북에 매칭하고, 플레이북의 단계들을 할 일 목록(todo list)에 복사한 뒤, 각 단계에 필요한 다른 스킬들을 순차적으로 실행합니다. 건너뛰는 단계는 목록에 `skip: <사유>`로 남습니다. 좋은 프롬프트는 달성할 목표와 완료 여부를 확인하는 방법을 명시합니다. 플레이북이 유지해야 할 단계를 빠뜨리거나 순서를 뒤섞기 쉬우므로 프롬프트에 실행할 스킬들을 손수 나열하지 마세요. 프롬프트 문구 작성을 도울 때는 먼저 [`references/prompting.md`](references/prompting.md)를 읽으세요. [가이드 2페이지](../../docs/guide/02-poteto-mode.md)에 예시들이 나와 있습니다.

`/poteto-mode`가 계속 켜져 있는지 여부는 사용자가 어떻게 시작하느냐에 달려 있습니다:

- `/poteto-mode`를 입력하고 그냥 Enter를 누르면 해당 메시지 하나에만 스킬이 첨부됩니다. 대화가 길어지면 효과가 점차 사라집니다.
- Mac에서 Option+Enter, Windows에서 Alt+Enter를 누르거나 스킬 항목에서 Use as Mode를 선택하면 Custom Mode가 됩니다. 이 경우 사용자가 모드를 종료할 때까지 매 턴마다 컨텍스트에 유지되며, 가벼운 일반 대화 턴에는 개입하지 않습니다.
- Cursor 문서에 따르면 Custom Modes는 Agents Window와 CLI에서 지원됩니다. 그 외 환경에서는 새 과업을 시작할 때마다 `/poteto-mode`를 붙여 시작하세요.

이 주제가 나오면 [Cursor 스킬 문서](https://cursor.com/docs/skills)를 링크해 주세요. 대화 도중 "새 작업(new task)"이라고 말하면 모드가 새로운 플레이북을 다시 매칭합니다. `/poteto-mode`는 플레이북 단계가 띄우는 서브에이전트에 이미 `poteto-agent`를 사용합니다. 사용자가 직접 띄우는 서브에이전트에서도 동일한 스타일을 얻으려면 `subagent_type: "poteto-agent"`로 띄우세요.

## 스킬 고르기 (Pick a skill)

기본 정답은 `/poteto-mode`이며, 각 단계에서 필요할 때 다른 대부분의 스킬들을 알아서 실행합니다. 사용자가 플레이북이 기본으로 제공하는 것보다 특정 작업을 더 깊게 하거나 덜 하고 싶어 할 때만 개별 스킬 이름을 직접 안내하세요. 스킬을 추천하기 전에 먼저 해당 스킬 문서를 읽고, 예시 프롬프트를 하나 함께 제시하세요.

| 사용자가 원하는 작업 | 스킬 |
|---|---|
| 비자명한(non-trivial) 과업을 엄격하게 수행하기 | [`/poteto-mode`](../poteto-mode/SKILL.md) |
| 작업을 시작하기 전에 에이전트가 목표와 문제를 제대로 이해했는지 확인하기 | [`/in-your-own-words`](../in-your-own-words/SKILL.md) |
| 프롬프트를 보내기 전에 길이만 늘리는 것이 아니라 더 깊이 있게 다시 쓰기(최고의 전문가라면 무엇을 추가할지, 어떤 원자료와 방법론을 쓸지, 외부 검증 기준은 무엇인지 추가. 수정된 프롬프트만 반환하며 실제 과업은 수행하지 않음) | [`/sharpen`](../sharpen/SKILL.md) |
| 현재 코드가 어떻게 동작하는지, 또는 새 코드를 어디에 두어야 할지 파악하기 | [`/how`](../how/SKILL.md) |
| 코드가 왜 이런 형태를 띠고 있는지, 또는 특정 수치가 어디서 나왔는지 파악하기 | [`/why`](../why/SKILL.md) |
| 변경사항이나 서브시스템을 평이한 설명으로 이해하기 | [`/teach`](../teach/SKILL.md) |
| 특정 주제에 대해 최근 자신이 했던 작업 내역 다시 파악하기 | [`/recall`](../recall/SKILL.md) |
| 마지막 메시지 이후 이 채팅에서 무엇이 바뀌었는지 평이한 단어와 작은 시각 자료로 따라잡고, 결정을 기다리는 모든 사안을 응답만 읽고도 즉시 결정할 수 있게 정리 받기 | [`/ketchup`](../ketchup/SKILL.md) |
| `/ketchup`처럼 따라잡되, 자막이 달린 짧은 슬라이드 비디오와 그 아래 텍스트로 정리된 결정 사안 목록으로 보기 | [`/ketchup-video`](../ketchup-video/SKILL.md) |
| 작은 diff가 그 외부의 어떤 부분을 깨뜨릴 수 있는지 알아내기 | [`/blast-radius`](../blast-radius/SKILL.md) |
| 함수 경계를 넘나드는 코드를 작성하기 전에 타입과 모듈 형태부터 확정하기 | [`/architect`](../architect/SKILL.md) |
| 하나의 요구사항에 대해 여러 시도를 병렬로 생성한 뒤 장점만 합쳐 최선의 결과물 얻기 | [`/arena`](../arena/SKILL.md) |
| 클라우드 에이전트들을 띄워 여러 조각에 대해 병렬 검사를 실행하거나 워커들을 경쟁시키기 | [`/swarm`](../swarm/SKILL.md) |
| 서로 다른 모델들이 diff를 리뷰하며 깨뜨려 보게 하기 | [`/interrogate`](../interrogate/SKILL.md) |
| 저렴한 로컬 테스트가 있을 때 테스트 우선(test-first)으로 버그 수정하기 | [`/tdd`](../tdd/SKILL.md) |
| `.ts` 또는 `.tsx` 작업에 TypeScript 규칙 적용하기 | [`/typescript-best-practices`](../typescript-best-practices/SKILL.md) |
| 리뷰 전에 코드를 작성하지 않은 별도 리뷰어를 사용해 불필요한 주석 걷어내기 | [`/no-comments`](../no-comments/SKILL.md) |
| 산문에서 AI 특유의 상투적 문투(AI tells) 청소하기 | [`/unslop`](../unslop/SKILL.md) |
| 문서, RFC, README, PR 설명, 커밋 메시지를 높은 기준에 맞춰 작성하기 | [`/technical-writing`](../technical-writing/SKILL.md) |
| 방금 한 마지막 응답을 평이한 인간의 언어로 다시 듣기 | [`/bro`](../bro/SKILL.md) |
| 에이전트가 앱을 구동하고 동작을 증명할 수 있는 스크립트화된 방법 만들어 주기 | [`/create-verification-skill`](../create-verification-skill/SKILL.md) |
| 검증 스킬과 기능 맵을 현재 앱 상태에 맞게 다시 일치시키기 | [`/maintain-verification-skill`](../maintain-verification-skill/SKILL.md) |
| 일반 테스트가 좀처럼 닿지 못하는 경쟁 상태(race), 불변식, 기타 속성을 정형 검증하기 | [`/create-formal-verification-skill`](../create-formal-verification-skill/SKILL.md) |
| 커버리지 대상 코드가 바뀐 후 정형 검증을 다시 코드와 일치시키고, 실패 원인이 드리프트인지 실제 버그인지 판별하기 | [`/maintain-formal-verification-skill`](../maintain-formal-verification-skill/SKILL.md) |
| PM이나 디자이너에게 사용자가 보게 될 화면 흐름을 TLA+ 모델로 검증된 차트(규칙 위반 상태에 도달하는 최단 경로 포함)로 보여주기 | [`/statechart`](../statechart/SKILL.md) |
| 성능 수치를 보고하거나 그에 따라 행동하기 전에 철저히 검증하기 | [`/benchmark-checklist`](../benchmark-checklist/SKILL.md) |
| 대규모 변경이나 횡단 관심사 변경, 또는 자리를 비운 뒤 사후에 리뷰할 작업 실행하기 | [`/figure-it-out`](../figure-it-out/SKILL.md) |
| 실행 도중 의사결정 로그를 기록하고 사후에 리뷰하기 | [`/show-me-your-work`](../show-me-your-work/SKILL.md) |
| 각 역할별 모델과 추론 예산 선택하기 | [`/setup-pstack`](../setup-pstack/SKILL.md) |
| 자신의 작업 습관을 개인화된 모드 스킬로 만들기 | [`/automate-me`](../automate-me/SKILL.md) |
| 완료된 과업에서 배운 교훈을 스킬 편집으로 반영하기 | [`/reflect`](../reflect/SKILL.md) |
| 이 저장소에서 에이전트들이 동일한 실수를 반복하지 못하게 원천 차단하기 | [`/correct`](../correct/SKILL.md) |
| 버튼을 누르면 웹훅으로 Grok Bot을 깨우는 웹 페이지 만들기 | [`/make-bot-ui`](../make-bot-ui/SKILL.md) |
| pstack 전반의 사용법과 길 찾기 | `/poteto-help` |

만약 이 스킬 옆에 있는 스킬 디렉터리가 위 표에 빠져 있다면, 해당 스킬의 frontmatter를 읽고 `description`에 따라 안내하세요. `principle-*` 디렉터리들은 아래 원칙(principles) 섹션에서 다룹니다.

헷갈리기 쉬운 스킬 비교 (Close calls):

- `/how`는 코드가 무슨 일을 하는지 설명합니다. `/why`는 왜 그렇게 되어 있는지 이유를 설명합니다. `/teach`는 둘 중 하나 또는 둘 다를 실행한 뒤 그 결과를 평이하게 풀어서 가르쳐 줍니다.
- `/arena`는 모든 워커에게 동일한 요구사항을 주고 가장 훌륭한 부분들을 병합합니다. `/swarm`은 작업을 여러 조각으로 나누거나 경쟁(race)시키고 단일 보고서를 반환합니다.
- `/architect`는 설계를 확정한 직후 곧바로 구현까지 진행합니다. 코드를 작성하기 전에 설계를 먼저 리뷰하고 싶다면 "with checkpoint"를 덧붙이세요.
- `/interrogate`는 diff 자체를 적대적으로 리뷰합니다. `/blast-radius`는 diff 바깥에서 발생할 수 있는 파손을 찾고 변경사항이 안전한 결정적 사실 한 가지를 증명합니다.
- `/recall`은 최근의 여러 채팅에 걸쳐 컨텍스트를 재구축합니다. `/ketchup`은 오직 현재 채팅에서 사용자의 마지막 메시지 이후에 일어난 일만 다룹니다. 특정 채팅이나 브랜치 하나를 이어받아 재개하는 것은 Session pickup 플레이북의 역할입니다.
- `/figure-it-out`은 하나의 엄격한 실행 워크플로를 설계합니다. Orchestrate 플레이북은 수일에 걸쳐 여러 PR로 이어지는 대규모 프로그램을 실행합니다. Autonomous run 플레이북은 단일 과업을 완료 조건까지 자율 구동합니다.

pstack에 포함되지 않은 기능 (Not in pstack):

- `/deslop`, `control-cli`, `control-ui`는 `cursor-team-kit` 플러그인에 포함되어 있습니다.
- `/loop`와 `/create-skill`은 Cursor 내장(built-in) 기능입니다.
- pstack에는 `/orchestrate`라는 개별 스킬이 없습니다. Orchestrate는 `/poteto-mode` 내부의 플레이북입니다. 만약 슬래시 메뉴에 `/orchestrate`가 보인다면 다른 플러그인이 제공하는 것입니다.

## 플레이북과 원칙 (Playbooks and principles)

플레이북은 개별 스킬이 아니라 `/poteto-mode` 내부에 들어 있는 단계 목록이므로 별도의 슬래시 명령어가 없습니다. `/poteto-mode` 안에서 과업을 설명하면 적절한 플레이북이 자동 선택되며, 아래 문구들을 사용하면 특정 플레이북을 직접 지정할 수 있습니다:

- "babysit this pr" 또는 "check on pr 123"은 Babysit 플레이북을 실행합니다. PR을 머지 가능 상태(merge-ready)까지 끌어올리고 거기서 멈춥니다. 사용자가 명시적으로 merge, land, 또는 ship을 요청하지 않는 한 직접 머지하지 않습니다.
- "land the stack"은 Shipping 플레이북을 실행합니다.
- "take over this branch"는 Session pickup 플레이북을 실행합니다.
- "pause safely"는 Pause safely 플레이북을 실행합니다.
- "full autopilot on this queue"는 Autopilot-full 플레이북을 실행합니다. "autopilot, don't merge"는 Autopilot-stack을 실행합니다.
- "run the eval playbook"은 Eval 플레이북을 실행합니다.

`/poteto-mode` 없이 "babysit this pr" 같은 문구를 그냥 입력하면 동일한 작업을 위한 Cursor 자체 내장 스킬이 대신 실행될 수 있습니다. 모든 플레이북 목록과 적용 시점은 [`poteto-mode`](../poteto-mode/SKILL.md)의 Playbooks 섹션에 정리되어 있습니다. PR 열기, 돌보기(babysitting), 반영(landing)에 관한 자세한 내용은 [가이드 6페이지](../../docs/guide/06-verify-and-ship.md)를 참조하세요.

pstack에는 별도의 계획 전용 스킬이 없으며, Cursor의 Plan Mode가 그 역할을 함께 수행합니다. 여러 단계나 스택형 PR에 걸친 작업의 경우 `/poteto-mode`에게 계획을 요청하면 [Multi-phase plan 플레이북](../poteto-mode/playbooks/multi-phase-plan.md)이 실행되어 구현 없이 계획 문서만 작성합니다. 설계상의 질문이 있을 때는 Prototype 플레이북이나 `/architect`가 코드로 먼저 형태를 확정합니다.

원칙(Principles)은 `/poteto-mode`가 읽고 응답에서 인용하는 단일 규칙 스킬들입니다. 사용자가 직접 원칙 스킬을 호출하는 일은 드뭅니다. 대신 "apply prove it works. show me the real output.(작동 증명 원칙을 적용해. 실제 출력을 보여줘.)"처럼 원칙의 이름을 언급하여 방향을 조종(steer)합니다. 물론 `/principle-<name>`을 직접 입력해 온디맨드로 로드할 수도 있습니다. 전체 원칙 목록은 [가이드 8페이지](../../docs/guide/08-principles.md)에 나와 있습니다.

## 잘못된 실행 바로잡기 (Fix a run that went wrong)

| 증상 | 해결 방법 |
|---|---|
| 몇 턴이 지나자 모드가 더 이상 적용되지 않음 | 그냥 Enter를 눌러 시작했기 때문입니다. Custom Mode로 시작하거나 매 과업마다 `/poteto-mode`를 붙여 시작하세요. |
| 단순 질문을 던졌는데 이전 과업의 다음 단계로 취급됨 | "새 작업(new task)"이라고 말하거나 이번 턴에는 모드가 필요 없다고 말하세요. |
| 새로 선택한 모델 설정이 반영되지 않음 | `/setup-pstack`이 생성한 규칙은 새 채팅부터 적용됩니다. 새 채팅을 여세요. |
| 실행 비용이 예상보다 많이 나옴 | 위 '초기 설정하기' 섹션의 비용 관련 단락을 참조하세요. |
| 스킬이 자동으로 로드되지 않음 | 사용자의 일반 발화에서 자동 로드되는 것은 `/setup-pstack`뿐입니다. 나머지 스킬들은 사용자가 직접 입력하거나 `/poteto-mode`가 호출할 때 로드되며, `/poteto-mode`도 모든 스킬을 다 실행하지는 않습니다. |
| 병렬 에이전트들이 서로의 파일을 덮어씀 | 각 에이전트에게 고유한 워크트리를 부여하거나, 각자 독립된 머신을 할당받는 클라우드 에이전트로 실행하세요. |
| 야간 자율 실행(overnight run)이 돌아가긴 했지만 아무것도 끝내지 못함 | `/loop`에는 단순 실행 시간이 아니라 통과/실패를 판정할 수 있는 검사 기준이 필요합니다. [가이드 7페이지](../../docs/guide/07-overnight.md)를 참조하세요. |
| 빌드가 그린이라는 이유만으로 성공했다고 주장함 | 실제 명령어 실행 결과, 화면 흐름, 저장된 값, 또는 프로파일링 결과를 요구하세요. 그것이 바로 prove-it-works 원칙입니다. |

실행이 궤도에서 벗어날 때 쓸 수 있는 한 줄 조종 문구(one-line steers)는 [`references/prompting.md`](references/prompting.md)에 정리되어 있습니다. 더 많은 함정 사례와 복사해 쓸 만한 레시피들은 [가이드 10페이지](../../docs/guide/10-recipes-and-pitfalls.md)에 나와 있습니다.

## pstack을 내 스타일에 맞게 커스터마이징하기 (Make pstack my own)

- [`/automate-me`](../automate-me/SKILL.md)는 사용자의 작업 히스토리를 바탕으로 `/poteto-mode`와 함께 사용할 개인 모드 스킬 초안을 작성해 줍니다.
- 작업 세션이 끝난 후 [`/reflect`](../reflect/SKILL.md)를 실행하면 그 세션에서 얻은 교훈을 사용자가 승인할 수 있는 스킬 편집안으로 변환합니다.
- `/poteto-mode write a skill for <workflow>`를 입력하면 스킬 작성(authoring) 플레이북이 실행됩니다. Eval 플레이북은 스킬 변경사항을 블라인드 테스트합니다.
- 오동작하는 스킬은 문제가 발생한 기능 개발 PR 안이 아니라 별도의 독립된 PR에서 수정하세요.

이에 대한 자세한 내용은 [가이드 9페이지](../../docs/guide/09-make-it-yours.md)에서 다룹니다.

## 응답 방식 (Reply)

결론부터 먼저 말하세요. 코드 블록 안에 예시 프롬프트를 최대 하나만 제시하고(적합한 것이 있다면 [`references/recipes.md`](references/recipes.md)에서 가져와 상황에 맞게 변형), 이어서 관련 파일의 링크를 제공합니다. 사용자가 전체 맵을 보여달라고 요청하지 않는 한 응답은 짧게 유지하세요.

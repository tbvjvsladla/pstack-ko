# 나만의 것으로 만들기

poteto-mode는 한 사람의 작업 스타일입니다. 하지만 그 아래에 있는 플레이북, 라우팅, 모델 역할 구조는 여러분의 스타일과도 똑같이 잘 작동합니다. 이 페이지에서는 개인 맞춤형 모드 생성하기, 세션에서 얻은 교훈 갈무리하기, 에이전트가 같은 실수를 반복하지 않도록 저장소 고치기, 집중된 스킬 작성하기, 그리고 스킬 변경 사항을 신뢰하기 전에 테스트하기를 다룹니다.

생각보다 더 작게 시작하세요. 첫날부터 수많은 스킬이나 이 플러그인 전체가 다 필요한 것은 아닙니다. 평이하게 프롬프트를 주고, 에이전트가 어디서 실패하는지 관찰한 뒤, 동일한 실패가 두 번 나타날 때 스킬이나 검사를 추가하세요.

## `/automate-me`로 나만의 모드 생성하기

```text
/automate-me
```

자신의 스타일을 직접 설명할 필요가 없습니다. [`/automate-me`](../../skills/automate-me/SKILL.md)가 여러분의 작업 기록에서 스타일을 읽어내기 때문입니다. 현재 워크스페이스의 최근 트랜스크립트를 마이닝하여 응답 방식, 위임, 검증, 코드, 산문, 프로세스 전반에서 반복되는 선호도를 찾아낸 뒤, 어떤 패턴이 진짜 여러분의 스타일인지 물어봅니다. 그런 다음 Cursor의 내장 `create-skill` 플로우를 통해 `.cursor/skills/<your-name>-mode/SKILL.md` 초안을 작성하고, [`/unslop`](../../skills/unslop/SKILL.md)으로 다듬은 뒤, 다른 변경 사항과 똑같이 리뷰할 수 있도록 워크트리에서 PR을 엽니다.

작업 습관이 바뀌었을 때는 언제든 다시 실행하세요:

```text
/automate-me update my mode skill with everything since its last edit
```

업데이트 모드는 스킬이 마지막으로 수정된 이후의 기록만 마이닝합니다. 기존과 상충하지 않는 규칙은 유지하고, 새로운 증거가 나온 규칙은 개정하며, 새로운 패턴에 대해서만 섹션을 추가합니다.

## `/reflect`로 세션의 교훈 갈무리하기

무언가 배운 점이 있는 과업을 마친 직후에 다음을 실행하세요:

```text
/reflect that took way too long. capture what we learned so the next run doesn't repeat it.
```

[`/reflect`](../../skills/reflect/SKILL.md)는 트랜스크립트를 세 명의 병렬 리뷰어에게 보낸 뒤, 종합기(synthesizer)가 제안들을 `Accepted`, `Rejected`, `Backlog`로 분류하고 실제 스킬을 수정하기 전에 여러분의 승인을 기다립니다. 향후 의사결정을 실제로 바꿀 수 있는 제안만 승인하세요. 한 번 겪은 특이한 세션은 일화(anecdote)일 뿐 규칙이 아닙니다.

## `/correct`로 환경 고치기

에이전트의 똑같은 실수를 반복해서 교정하고 있다면, 그 해결책은 다음 프롬프트가 아니라 저장소 안에 들어가야 합니다. 강제력이 단단한 순서대로 해결책의 순위를 매기면 다음과 같습니다:

1. 아키텍처나 더 나은 자료구조를 통해 그 실수 자체를 불가능하게 만듭니다.
2. 타입으로 막거나, 에러 메시지에 해결 방법이 명시된 린트 또는 CI 검사로 차단합니다.
3. 테스트로 잡아냅니다.
4. 문서나 에이전트 규칙으로 적어 둡니다. 에이전트가 규칙을 건너뛰어도 아무것도 실패하지 않으므로 이 방법은 가장 마지막 수단입니다.

사람의 코드 리뷰는 이 목록에 없습니다. 매 PR마다 똑같은 실수를 사람이 직접 잡아내야 하는 상황 자체가 바로 이 작업이 고치려는 문제이기 때문입니다. [`/correct`](../../skills/correct/SKILL.md)가 그 일을 수행합니다:

```text
/correct agents keep calling the database client directly instead of going through the repository layer
```

최근 커밋, 리버트(revert), 리뷰 코멘트, 임시방편을 설명하는 주석들을 읽은 뒤 실수들을 유형(class)별로 묶습니다. 두 번 이상 발생한 유형만 계산에 넣습니다. 가장 빈번한 실수 유형들을 작동하는 가장 높은 수준에서 커밋당 하나씩 수정하고, 각각의 새로운 검사가 실제 과거 실수에 대해 실패함을 증명합니다. 또한 에이전트 지침 파일에 각 규칙과 그 강제 수단을 짝지은 표를 유지하므로, 아무것도 강제하지 않는 규칙은 반복 실수로 즉시 드러납니다. 응답에는 각 실수 유형과 그 증거, 선택된 강제 수준, 그리고 왜 그보다 더 높은 수준은 적용할 수 없었는지가 나열됩니다.

인자 없이 실행하면 히스토리에서 실수 유형들을 스스로 찾아냅니다. `/reflect`와 `/correct`는 역할을 분담합니다. `/reflect`는 단일 세션으로부터 스킬을 개선하고, `/correct`는 특정 실수 유형이 다시는 재발할 수 없도록 저장소를 바꿉니다. 해결책이 새로운 경계를 세우는 것일 때는 `/architect`와 짝지어 사용하세요. 저장소 전체에 대해 두 스킬을 함께 실행하는 프롬프트는 [여러 프로젝트를 병렬로 실행하기](./07-overnight.md#run-many-projects-in-parallel)에 나와 있습니다.

## 집중된 스킬 직접 작성하기

갈무리하고 싶은 워크플로우를 이미 명확히 알고 있을 때는 다음과 같이 요청하세요:

```text
/poteto-mode write a skill for verifying database migrations in this repo
```

스킬 작성 요청은 [Authoring or modifying a skill 플레이북](../../skills/poteto-mode/playbooks/authoring-a-skill.md)과 매칭되며, Cursor의 내장 `create-skill`을 거쳐 frontmatter와 링크를 검증하고 Opening a PR 플레이북을 통해 결과를 배포합니다. 에이전트가 읽을 산문은 사람이 읽을 산문보다 기준이 더 까다로운데, 도움이 되지 않는 문장 하나가 향후 어떤 에이전트가 그대로 따를 지침이 되어버리기 때문입니다. `SKILL.md`를 백지에서 수작업으로 작성하는 대신 플레이북이 그 품질 기준을 붙잡게 하세요.

두 가지 특수한 경우에는 전용 생성기가 따로 있습니다. 앱을 직접 구동하고 동작을 증명해야 하는 스킬은 검증 스킬이므로 대신 [`/create-verification-skill`](../../skills/create-verification-skill/SKILL.md)과 [`/maintain-verification-skill`](../../skills/maintain-verification-skill/SKILL.md)을 사용하세요. 모델 체커, 수학적 증명 또는 기타 검증기로 코드의 속성을 검사하는 스킬은 정형 검증 스킬이므로 [`/create-formal-verification-skill`](../../skills/create-formal-verification-skill/SKILL.md)과 [`/maintain-formal-verification-skill`](../../skills/maintain-formal-verification-skill/SKILL.md)을 사용하세요. 두 종류 모두 [검증 및 배포하기](./06-verify-and-ship.md#prove-a-property-with-a-formal-verification-skill)에서 자세히 다룹니다.

## `/technical-writing`으로 표준에 맞춘 문서 작성하기

배포하는 산문이 스킬만 있는 것은 아닙니다. 문서, RFC, README, PR 설명, 커밋 메시지에는 다음을 사용하세요:

```text
/technical-writing review the readme changes
```

[`/technical-writing`](../../skills/technical-writing/SKILL.md)은 계층형 표준을 적용합니다. 목표는 지친 엔지니어가 한 번 읽고도 바로 이해할 수 있는 산문입니다. 먼저 문서의 모드(튜토리얼, 하우투, 레퍼런스 또는 설명)를 고른 뒤 문장 단위로 다듬습니다: 누가 무엇을 하는지 분명하게, 한 문장에는 하나의 생각만, 두 가지 의미로 읽힐 여지가 없게 만듭니다. 여러분이나 에이전트가 방금 작성한 글을 리뷰할 때 쓰거나, 문서를 요청할 때 처음부터 이 스킬을 지정하세요.

## 블라인드 방식으로 스킬 변경 테스트하기

스킬 편집은 향후 모든 세션에 영향을 미치므로 실험을 다루듯 테스트해야 합니다. 다른 사람의 스킬을 도입할 때도 마찬가지입니다. 스킬을 유지하기 전에 여러분의 실제 작업에서 에이전트를 더 낫게 만드는지 먼저 확인하세요.

```text
/poteto-mode run the eval playbook on this skill change. same task for both variants, candidates stay blind.
```

스킬이 계속 핵심을 놓치고 있고 어떻게 동작해야 하는지 알고 있다면 한 과업 안에서 수정과 평가를 함께 실행하세요:

```text
/poteto-mode update the review skill so it flags missing migrations, and eval the change.
```

처음부터 평가를 함께 요청하면 편집이 정직하게 유지됩니다. 한 번의 나쁜 세션만을 보고 작성한 수정은 그 세션에 과적합(overfit)되기 쉬우며, 여러 번의 편집이 쌓이면 스킬이 원래 의도에서 표류(drift)하게 됩니다. 평가는 그러한 표류를 배포 전에 잡아냅니다.

[Eval 플레이북](../../skills/poteto-mode/playbooks/eval.md)은 관찰자 효과(observer effect)라는 단일 실패 모드를 막기 위해 설계되었습니다. 자신이 평가받고 있다는 사실을 아는 에이전트는 평소와 다르게 행동합니다. 따라서 후보 에이전트들은 흔적이 제거된 디렉터리에서 자연스러운 실제 과업처럼 보이는 작업을 부여받으며, "eval"이나 "candidate"라는 단어나 다른 후보의 존재를 절대 알지 못합니다. 한 명의 심사위원이 중립적인 레이블 아래 모든 출력을 채점하며, 스킬 체인 준수 여부는 후보의 자체 주장이 아니라 실제로 어떤 파일들을 읽었는지를 기준으로 채점됩니다.

판정을 수용하기 전에 모든 출력을 직접 읽어보세요. 심사위원의 판정에 동의할 수 없다면 여러분의 판단력을 의심하기 전에 루브릭을 먼저 의심하세요.

## `/make-bot-ui`로 봇 UI 만들기

Grok Bot 사용자를 위한 상황별 스킬도 하나 있습니다. [`/make-bot-ui`](../../skills/make-bot-ui/SKILL.md)는 웹훅 루틴을 통해 봇을 깨우는 버튼이 달린 작은 페이지를 만들어 줍니다. 예를 들어 리뷰 큐를 스와이프하면서 각 스와이프가 봇에게 해당 항목을 처리하도록 요청하게 만들 수 있습니다. 로컬 머신의 서버가 웹훅의 발신자 키(sender key)를 보관하므로 키가 브라우저나 채팅에 노출되는 일은 없습니다. 또한 이 스킬은 페이지를 Tailscale로 노출하는 방법도 다룹니다.

**함정(Pitfall):** 과업 도중 스킬이 말을 안 듣는다고 해서 그 자리에서 스킬을 편집하지 마세요. 스킬 수정은 별도의 PR에서 처리하고 현재 과업은 계속 진행시키세요. 기능 구현 작업에 뒤엉켜 배포되는 스킬 편집은 리뷰에서 눈에 띄지도 않고 평가(eval)할 수도 없습니다.

다음: [레시피 및 흔한 함정](./10-recipes-and-pitfalls.md).

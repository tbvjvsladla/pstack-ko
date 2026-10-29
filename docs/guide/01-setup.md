# pstack 설정하기

이 페이지에서는 플러그인을 설치하고, pstack이 사용할 모델을 고른 뒤, 첫 번째 과업을 실행합니다. 설정은 명령어 하나와 짧은 대화면 끝납니다.

## 플러그인 설치하기

Cursor 채팅에서 다음을 실행하세요:

```text
/add-plugin pstack
```

Cursor가 플러그인이 설치되었음을 확인해 줍니다.

Claude Code에서는 `/plugin marketplace add cursor/plugins`를 실행한 다음 `/plugin install pstack@cursor-plugins`를 실행하세요. Claude Code는 플러그인 명령어 앞에 접두사를 붙이므로, 이 가이드에 나오는 각 명령어는 그곳에서 `/pstack:setup-pstack`처럼 `pstack:` 접두사를 갖게 되며 모델 규칙은 `~/.claude/rules/pstack-models.md`에 기록됩니다. 그 밖의 차이점은 [Claude Code 어댑터](../harness/claude-code.md)에서 다룹니다.

## 모델 선택하기

다음을 실행하세요:

```text
/setup-pstack
```

[`/setup-pstack`](../../skills/setup-pstack/SKILL.md)은 접근 가능한 모델을 감지하고, 추론 예산(reasoning budget)을 물어본 뒤, 각 역할(코드 위임 에이전트, 판단, 리뷰 패널)을 보여주고 원하는 구성을 묻습니다. 질문에 답하면 모든 pstack 스킬이 읽는 작은 규칙 파일인 `~/.cursor/rules/pstack-models.mdc`를 작성합니다.

기본값은 `large` 예산과 동일한 `xhigh` 추론 수준으로 실행됩니다. `unlimited`는 각 모델을 최대 `max`까지 가장 높은 티어로 끌어올립니다. Opus는 `max`로 올라가고, Grok은 최대치가 `xhigh`이므로 그대로 유지됩니다. `medium`과 `small`은 추론 수준을 낮춰 토큰을 덜 소비합니다.

신경 쓰이는 역할만 오버라이드하면 됩니다. 규칙 파일에 해당 줄이 없는 역할은 스킬의 기본값을 유지합니다. 기본값으로 되돌리려면 해당 역할 줄을 삭제하세요. `/setup-pstack`을 다시 실행하면 기본값과 다른 모델이 지정된 역할은 그대로 유지됩니다. 기본값이 변경되더라도 변경 전에 작성된 규칙 파일은 여전히 예전 기본값을 고정(pin)하므로, 해당 역할 줄을 삭제하거나 파일을 삭제한 뒤 `/setup-pstack`을 다시 실행하세요.

Auto를 쓰면 어떻게 되는지 궁금할 수 있습니다. 역할을 `inherit-parent` 또는 `auto`로 설정하면 pstack은 서브에이전트의 `model` 필드를 생략하므로, 서브에이전트가 부모 채팅의 모델을 그대로 물려받습니다. 두 값은 동일한 의미이며 둘 다 모델 슬러그가 아닙니다. 패널 역할의 경우 값은 리스트 형태이며 항목당 하나의 서브에이전트가 실행되므로 리스트 길이가 곧 패널 크기를 결정합니다. 또한 셋업에서는 경합(race)에서 각 부문에 모델을 별도로 지정하지 않았을 때 모든 `/swarm` 워커에 쓰이는 기본 모델인 `swarm workers`도 설정합니다.

## 검증 스킬 생성 제안 수락하기 (또는 건너뛰기)

셋업 마지막에 `/setup-pstack`은 프로젝트에서 앱 동작을 증명할 수단(`verify-*` 스킬 또는 기존 하네스)이 있는지 찾아봅니다. 둘 다 찾지 못하면 [`/create-verification-skill`](../../skills/create-verification-skill/SKILL.md)로 검증 스킬을 생성할지 한 번 제안합니다.

예(yes)라고 답하면 에이전트에게 사용자처럼 앱을 구동하는 법을 가르치는 프로젝트 로컬 스킬인 `.cursor/skills/verify-<app>/`을 작성합니다. 그리고 인계하기 전에 스킬이 실제로 작동하는지 한 번 증명해 보입니다. 아니오(no)라고 답하면 셋업은 다음으로 넘어갑니다. 언제든 직접 `/create-verification-skill`을 실행할 수 있습니다. 자세한 내용은 [검증 및 배포하기](./06-verify-and-ship.md#create-a-project-verification-skill)에서 깊이 있게 다룹니다.

pstack이 처음이라면 예(yes)라고 답하세요. 스스로 작업을 검증할 수 있는 에이전트는 검증이 통과할 때까지 계속 나아갑니다. 그렇지 못한 에이전트는 매 결과를 여러분이 직접 수작업으로 확인하도록 되돌려보냅니다. 이 가이드의 모든 내용 중 검증 스킬이 가장 큰 보상을 돌려줍니다.

그다음 셋업은 프로젝트에 이미 정형 검증 스킬이 없는 한 [`/create-formal-verification-skill`](../../skills/create-formal-verification-skill/SKILL.md)로 정형 검증 스킬을 생성할지 한 번 제안합니다. 일반 테스트는 누군가 미처 생각해서 작성한 케이스만 확인합니다. 정형 검증 스킬은 "작업(job)이 동시에 두 번 실행되는 일은 절대 없다"와 같은 코드의 핵심 속성 하나를 명시된 범위 내의 모든 케이스에 대해 검사합니다. 동시성, 재시도, 상태 머신 또는 파서처럼 버그 비용이 크고 일반 테스트가 깨지는 케이스에 좀처럼 도달하지 못하는 코드가 있다면 예(yes)라고 답하세요. 그렇지 않다면 아니오(no)라고 답하고 나중에 상황이 바뀔 때 실행하면 됩니다. 자세한 내용은 [검증 및 배포하기](./06-verify-and-ship.md#prove-a-property-with-a-formal-verification-skill)에서 다룹니다.

셋업이 끝나면 새 채팅을 시작하세요. 모델 규칙은 새 세션부터 적용됩니다.

## 비용 관리하기

pstack은 서브에이전트와 리뷰 패널에 추가 토큰을 소비합니다. 그것이 엄격함의 대가입니다. 토큰 소비를 줄이려면 다음 방법을 활용하세요:

- `/setup-pstack`을 다시 실행하여 더 작은 추론 예산이나 더 저렴한 모델을 선택합니다. 메인 채팅에는 강력한 모델을 두고 코드 역할에는 더 저렴하고 빠른 모델을 배치하는 것이 좋은 분담 방식입니다.
- 역할을 `auto` 또는 `inherit-parent`로 설정하여 채팅 자체 모델에서 실행되게 합니다.
- 패널 리스트를 줄입니다. 각 항목마다 서브에이전트 하나가 실행됩니다.
- 엄격함이 필요한 작업에만 `/poteto-mode`를 아껴 둡니다. 작고 자명한 편집에는 필요하지 않습니다.

## 첫 번째 과업 실행하기

실제 작업이면서 규모가 작은 것을 하나 골라 동료에게 설명하듯이 적어보세요:

```text
/poteto-mode add a --json flag to this command. text output stays byte-identical. verify both.
```

할 일 목록(todo list)을 지켜보세요. 첫 항목들은 매칭된 플레이북(이 프롬프트의 경우 Feature 플레이북)의 단계들이 그대로 복사되어 들어갑니다. `/poteto-mode`가 특정 단계를 건너뛰면 해당 단계는 `skip: <reason>`과 함께 목록에 남아 있으므로 무엇을 하지 않기로 선택했는지 눈으로 확인할 수 있습니다.

여기서부터는 일반적인 후속 지시를 입력하면 됩니다. 채팅 전체에서 `/poteto-mode`를 계속 켜두려면 `/` 메뉴에서 선택할 때 Enter 대신 Option+Enter(Mac) 또는 Alt+Enter(Windows)를 누르세요. 그러면 모드를 종료할 때까지 매 턴마다 컨텍스트에 유지되는 [커스텀 모드(Custom Mode)](https://cursor.com/docs/skills#using-a-skill-as-a-custom-mode)가 됩니다. 커스텀 모드는 에이전트 창(Agents Window)과 CLI에서 사용할 수 있습니다. 그냥 Enter를 누르면 해당 메시지 하나에만 스킬이 첨부되며 대화가 진행됨에 따라 영향력이 흐려집니다.

다음: [`/poteto-mode`를 통해 작업 라우팅하기](./02-poteto-mode.md).

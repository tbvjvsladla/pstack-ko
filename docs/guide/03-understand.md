# 변경하기 전에 코드 이해하기

이해하지 못한 코드를 편집하는 것은 미묘한 회귀(regression) 버그가 배포되는 지름길이며, 이는 여러분에게나 에이전트에게나 마찬가지입니다. 에이전트는 대개 두 가지 이유 중 하나로 실패합니다. 여러분이 원하는 바를 잘못 읽었거나, 작업을 제대로 수행할 맥락이 부족한 경우입니다. 첫 번째 문제는 [프롬프트에 담을 내용](./02-poteto-mode.md#what-goes-in-a-prompt)에서 다뤘고, 이 페이지에서는 두 번째 문제를 다룹니다.

pstack은 네 가지 진입점을 제공합니다. `/how`는 코드가 지금 무슨 일을 하는지 설명합니다. `/why`는 코드가 왜 그런 모양을 하게 되었는지 이유를 파헤칩니다. `/teach`는 둘을 엮어 하나의 설명으로 풀어냅니다. `/recall`은 특정 주제에 대한 여러분 자신의 최근 맥락을 재구성합니다. 또한 이 네 가지는 모두 에이전트가 자신의 이해를 여러분이 검증할 수 있는 말로 설명하게 만듭니다. 그것이 바로 여러분보다 코드를 더 잘 알 수도 있는 에이전트를 감독하는 방법입니다.

![A detective studies a machine blueprint with a magnifying glass while robots fetch case files; the evidence board behind her links clues under /how and /why.](./images/understanding.jpg)

## 읽기 전용 조사로 시작하기

원인이 불분명할 때는 수정이 아니라 발견 사항(findings)을 요청하세요:

```text
/poteto-mode investigate why background jobs time out every few hours. give me what we know, what data you used, and your best hypotheses. don't change any code yet.
```

"don't change any code yet"은 이 요청을 [Investigation 플레이북](../../skills/poteto-mode/playbooks/investigation.md)으로 라우팅합니다. 이 플레이북은 `/how`를 실행하고, 동기에 관한 질문에는 `/why`를 덧붙여 출처가 인용된 설명을 반환합니다. 여러 선택지 중 하나를 골라야 하는 질문에는 트레이드오프 표와 함께 추천안을 반환합니다. "what data you used(어떤 데이터를 사용했는지)"를 물어보면 에이전트가 증거와 추측을 분리하게 됩니다. 발견 사항이 명확한 수정 방향을 가리키면 그때 새로운 과업으로 수정을 시작하세요.

## `/how`로 동작 추적하기

```text
/how do we dedupe notifications? is there an n+1 when we look up subscribers?
```

실제로 궁금한 질문을 그대로 던지세요. [`/how`](../../skills/how/SKILL.md)는 코드를 읽고 시니어 엔지니어가 여러분에게 서브시스템을 온보딩해 주는 수준으로 런타임 흐름, 핵심 타입, 그리고 자명하지 않은 부분들을 짚어 답해줍니다. 큰 서브시스템의 경우 먼저 2~4개의 읽기 전용 탐색 에이전트를 병렬로 띄웁니다. 좁은 범위의 질문에는 바로 코드를 읽고 설명합니다.

## `/why`로 히스토리 파헤치기

```text
/why was the retry limit set to five? does the reason still hold?
```

[`/why`](../../skills/why/SKILL.md)는 소스 컨트롤에서 출발한 뒤 이슈 트래커, 장문 문서, 팀 채팅, 관측성 도구, 에러 트래킹, 애널리틱스 등 MCP가 노출하는 모든 증거 범주를 병렬로 질의합니다. 보고서는 모든 출처를 인용하고, 직접적인 증거와 추론을 분리하며, 기록이 빈약할 때는 "~인 것으로 보입니다(appears to)"라고 표현합니다. "아무도 이유를 적어두지 않았다"는 사실 자체가 하나의 답이므로 결과가 없는 경우(null result)도 그대로 보고됩니다.

이 두 스킬은 자연스럽게 결합됩니다. 복잡한 코드 뒤에 숨은 히스토리가 원인을 설명해 줄 것 같다면 `do why first then how`가 좋은 프롬프트가 됩니다.

## `/teach`로 진짜로 이해하기

```text
/teach me how this PR changes retries. convince me it fixes the cause and not the symptom.
```

[`/teach`](../../skills/teach/SKILL.md)는 단순 요약만으로는 부족할 때 씁니다. `/how`와 `/why`(작은 변경의 경우 둘 중 하나만)를 실행하고, 발견 사항들을 다이어그램을 하나씩 쌓아 올리는 평이한 설명 하나로 엮어냅니다. "convince me(나를 납득시켜 봐)"라는 프레이밍은 자주 재사용할 가치가 있습니다. 단순한 코드 유람을 여러분이 검증해 볼 수 있는 논증으로 바꿔주기 때문입니다.

에이전트 자신이 내린 선택에 대해서도 똑같이 작동합니다:

```text
/teach me why you implemented it this way and not with a queue. what did you trade off, and why?
```

가르치는 과정은 여러분만큼이나 에이전트에게도 도움이 됩니다. 자신의 작업을 설명해야 하는 에이전트는 자신만만하게 단언하고 넘어가는 대신 코드를 다시 읽고 각 주장을 뒷받침할 증거를 찾아야만 하기 때문입니다.

## `/recall`로 내 맥락 재구성하기

```text
/recall catch me up on the export work from last week
```

[`/recall`](../../skills/recall/SKILL.md)은 여러분의 최근 채팅 기록과 공유 기록(이슈, 이전 수정 사항, 여전히 발생 중인 에러)을 마이닝하여 현재 상황과 다음 할 일에 대한 브리핑을 돌려줍니다. 이전 채팅에는 새로운 에이전트가 모르는 맥락이 담겨 있으므로, 예전 주제에 대해 새 작업을 시작할 때는 먼저 맥락을 불러온 뒤 새로운 입력을 건네세요:

```text
/recall my work on the virtualized list from yesterday, then read this bug report.
```

특정 채팅 하나를 그대로 재개하고 싶다면 그것은 `/recall`이 아니라 아래의 Session pickup 플레이북이 담당합니다.

## Session pickup으로 이전 작업 이어받기

다른 에이전트(또는 지난주의 여러분)가 작업 중이던 브랜치를 도중에 남겨두었을 때 사용합니다:

```text
/poteto-mode take over this branch. read the decision log, figure out what's done, and continue from there. don't redo finished work.
```

[Session pickup 플레이북](../../skills/poteto-mode/playbooks/session-pickup.md)은 이전 기록을 권위 있는 정본으로 취급합니다. 브랜치 상태와 의사결정 내역을 재구성하고, 재개 지점을 명시하며, 처음부터 모든 것을 다시 유도하는 대신 원래 목표에 비추어 물려받은 주장들을 검증합니다.

**함정(Pitfall):** "어차피 에이전트가 알아서 코드를 읽을 텐데"라며 이 페이지의 스킬들을 건너뛰지 마세요. 추적된 모델 없이 바로 편집부터 시작하는 에이전트는 가장 먼저 눈에 띄는 그럴듯한 지점에서 표면적 증상만 고치는 경향이 있습니다. 먼저 `/how`를 돌리는 편이 두 번째 버그를 마주하는 것보다 훨씬 저렴합니다.

다음: [변경 사항 설계하기](./04-design.md).

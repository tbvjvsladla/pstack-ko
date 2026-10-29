# 레시피 및 흔한 함정

바로 복사해서 쓸 만한 프롬프트들과 누구나 한 번쯤 겪는 실수들을 모았습니다. 경로와 완료 조건만 여러분의 상황에 맞게 바꿔 쓰세요. 레시피들은 의도적으로 격식 없이 작성되었습니다. 실무에서도 그렇게 타이핑하며, 스킬들은 그러한 표현에서도 의도를 정확히 읽어냅니다.

![She tastes a finished dish while robots cook from a recipe box, with pinned cards reading /how, /tdd, and /loop above the counter.](./images/recipes.jpg)

## 낯선 서브시스템 이해하기

```text
use /how first to understand how this initialization works. then use /why to figure out why it broke recently.
```

작동 원리가 먼저이고 히스토리가 두 번째입니다. 각 스킬의 보고서는 어떤 소스를 탐색했는지 알려주므로 답변이 무엇에 근거하고 있는지 알 수 있습니다.

## 코드를 건드리기 전에 잡음 많은 제보 재진술하기

```text
/poteto-mode read this thread. restate the underlying issue in your own words, in plain english. don't change any code yet.
```

오독이 있다면 재진술에서 드러나며, 거기서 바로잡는 데는 메시지 하나면 충분합니다. 에이전트가 자신의 파악 내용을 말하기 전까지는 여러분의 추측을 아껴 두세요. 플레이북 없이 재진술만 요청할 때는 `/in-your-own-words`를 씁니다.

## 프롬프트를 보내기 전에 날카롭게 다듬기

```text
/sharpen write the onboarding doc for the new billing service.
```

에이전트는 전문가라면 여러분이 요청하지 않았더라도 챙겼을 사항, 출발점으로 삼을 원재료, 방법론과 도구, 그리고 결과가 맞는지 확인하는 외부 검증 기준을 짚어줍니다. 그것을 요청하도록 다시 작성한 프롬프트를 돌려주며 과업 자체를 수행하지는 않습니다. 다시 쓴 프롬프트가 마음에 들면 그때 전송하세요.

## 고르기 전에 프로토타입 만들기

```text
/poteto-mode prototype a few options for the settings layout. put them behind a switcher and send me screenshots of each.
```

설명문이 아니라 실제로 돌아가는 결과물을 보고 고르세요. 에이전트는 프로토타입을 만드는 과정에서 레이아웃과 타이밍에 관한 열린 질문들에 스스로 답합니다.

## 확정된 설계를 계획서로 바꾸기

```text
/poteto-mode turn this design into a plan. small verifiable PRs, each with its own proof.
```

설계가 자리 잡은 후에만 요청하세요. 계획서 자체가 최종 산출물이며, 이를 실행할 플레이북 이름을 함께 명시합니다.

## 설계에 대해 두 번째 의견 구하기

```text
ask /arena for a second opinion on this thread and our approach
```

현재 설계안이 여러 후보 중 하나가 되며, 합성 결과는 패널이 더 나은 대안을 찾았는지 아니면 기존 접근법을 재확인했는지 알려줍니다. 되돌리기 비싼 설계 비용에 비하면 아주 저렴한 투자입니다.

## 독립적인 슬라이스들을 병렬로 점검하기

```text
/swarm check every package under packages/ against its check.sh. one worker per package. one report.
```

각 워커가 패키지 하나씩을 맡습니다. 부모 에이전트는 모든 슬라이스를 기다렸다가 날것의 워커 덤프 대신 하나의 `PASS`, `ISSUES`, 또는 `BLOCKED` 보고서를 반환합니다.

## 회의적인 시각으로 브랜치 리뷰하기

```text
/interrogate the whole branch, but skeptically. don't change anything yet. no nitpicks unless it's an actual bug or regression in behavior.
```

수식어들이 실질적인 역할을 합니다. "don't change anything yet"은 작업을 읽기 전용으로 유지하고, 사소한 지적(nitpick) 금지 규칙은 잡음을 사전에 걸러내어 검증된 발견 사항들이 여러분의 시간을 쓸 만한 가치를 갖게 해 줍니다.

## 실패하는 테스트를 통해 버그 수정하기

```text
/poteto-mode repro the duplicate write first. if there's a cheap test path, /tdd it. then fix and rerun.
```

"if there's a cheap test path(저렴한 테스트 경로가 있다면)"라는 단서가 중요합니다. 깨지기 쉬운 모킹(mock)을 억지로 뚫고 테스트를 작성하는 것은 실제 명령어를 실행하는 것보다 증명력이 떨어지며, 플레이북은 그렇다고 솔직히 말할 수 있습니다.

## 증거와 함께 제보 재현 및 수정하기

```text
/poteto-mode repro this with /verify-<app>. if it repros on main, fix it and show me a video as proof.
```

"if it repros on main"은 버그가 이미 사라진 경우 실행을 조기에 멈출 수 있게 해 줍니다. 비디오가 있으면 diff를 읽기도 전에 수정 결과를 눈으로 먼저 확인할 수 있습니다.

## 일반 테스트가 거의 닿지 못하는 속성 증명하기

```text
/create-formal-verification-skill the retry queue. a message must never be delivered twice.
```

명시된 범위 내의 모든 케이스에 대해 검사된 속성 하나, 코드가 이미 그 속성을 위반하고 있다면 코드의 용어로 된 반례, 그리고 이후 해당 코드가 바뀔 때마다 다음 에이전트가 실행할 프로젝트 스킬을 얻게 됩니다.

## 수치를 게시하기 전에 검증하기

```text
/benchmark-checklist vet this 40% speedup before it goes in the pr description
```

실행 횟수, 범위, 수치를 제한하는 병목 요인과 함께 더 빠름, 더 느림, 측정 가능한 차이 없음 또는 판단 불가 판정을 받게 됩니다.

## 똑같은 실수 반복 교정 끝내기

```text
/correct agents keep adding new config flags without registering them in the schema
```

다음 에이전트가 같은 실수를 저지를 수 없도록 아키텍처, 타입, 린트 또는 테스트 형태로 수정 사항이 저장소에 랜딩됩니다.

## 작업을 시작하지 않고 방법만 물어보기

```text
/poteto-help how do i get poteto-mode to stay on every turn?
```

답변, 바로 보낼 수 있는 프롬프트, 출처 링크를 받습니다. 그 프롬프트를 직접 보내기 전까지는 아무 작업도 실행되지 않습니다.

## 자리를 비운 동안 실행을 정직하게 유지하기

```text
im going to bed, keep going autonomously until every fixture passes. do not stop. keep a decision log i can audit in the morning.
```

전체 계약은 [야간 자율 실행 페이지](./07-overnight.md)에 있습니다. 과업과 완료 조건이 이미 대화 안에 들어가 있다면 이 짧은 형태만으로도 작동합니다.

## 표류하는 실행 방향 바로잡기

조율(steering) 프롬프트는 한 줄이면 됩니다:

```text
i said the goal is to repro. i did not ask for a fix yet.
```

```text
apply prove it works. show me the real output, not the build log.
```

```text
/unslop that, no emdashes
```

더 긴 말은 거의 필요하지 않습니다. 필요한 것은 정확한 이름뿐이며, [원칙 페이지](./08-principles.md)가 바로 그 어휘집입니다.

## 평이한 일상 언어로 다시 설명받기

```text
/bro
```

프롬프트는 이게 전부입니다. [`/bro`](../../skills/bro/SKILL.md)는 방금 메시지를 사람 대 사람으로 이야기하듯 전문 용어 없이 더 짧게 다시 설명해 줍니다. 응답이 기술적으로는 치밀하지만 정작 무슨 말인지 한눈에 들어오지 않을 때 쓰세요.

## 켜 두고 떠났던 채팅 현황 따라잡기

```text
/ketchup
```

[`/ketchup`](../../skills/ketchup/SKILL.md)은 마지막 메시지 이후 무엇이 바뀌었는지 평이한 말로 알려준 뒤, 여러분의 결정을 기다리는 각 사안을 추천 선택지, 증거, 위험 요소와 함께 나열합니다. 보고하는 동안 아무것도 변경하지 않습니다.

대신 요약 비디오로 보고 싶다면 [`/ketchup-video`](../../skills/ketchup-video/SKILL.md)를 실행하세요. 그동안 일어난 일을 짧은 자막 슬라이드 비디오로 보여주고, 결정을 기다리는 사안들은 그 아래에 텍스트로 나열해 줍니다.

## 흔한 함정 (The pitfalls)

- **프롬프트에 스킬들을 일일이 나열하기.** "use /how then /architect then /arena"처럼 적으면 플레이북이 이미 맞춰 둔 단계 순서가 뒤섞입니다. 목표와 제약 조건만 말하세요. 스킬 이름은 기본값을 오버라이드할 때만 언급합니다.
- **모호한 완료 조건.** "더 좋게 만들어줘"는 `/loop`에 확인할 기준을 전혀 주지 못합니다. 통과 또는 실패를 판별할 수 있는 명령어나 아티팩트를 지정하세요.
- **원인에 대한 본인의 가설부터 먼저 말하기.** 에이전트는 여러분이 가리킨 곳만 탐색하게 됩니다. 먼저 에이전트에게 문제를 재진술하게 한 다음 여러분의 직감을 공유하세요.
- **첫 번째 설계안을 그대로 채택하기.** 단 한 번의 시도는 모델이 처음 떠올린 형태에 갇히게 만듭니다. 프로토타입이나 `/architect`를 요청하고 증거를 바탕으로 고르세요.
- **추상적인 계획서만 다듬기.** 뒤에 코드가 없는 계획서를 적대적으로 리뷰해 봐야 현실에서는 절대 일어나지 않을 위험만 지어낼 뿐입니다. 열린 질문들은 프로토타입으로 먼저 해결한 뒤 실제로 만들어진 결과물을 리뷰하세요.
- **하나의 워크트리에서 병렬 에이전트 돌리기.** 서로의 파일을 덮어쓰게 되어 어느 에이전트가 무엇을 바꿨는지 아무도 알 수 없게 됩니다. 클라우드 에이전트로 실행하거나 "시도마다 고유한 워크트리 사용(own worktree per attempt)"을 지시하세요.
- **루프를 신뢰하기 전에 루프부터 돌리기.** 스스로 작업을 검증할 수 없는 루프는 미검증 작업물만 더 빠르게 만들어냅니다. 검증 스킬부터 먼저 작동하게 만드세요.
- **검증되지 않은 수치 신뢰하기.** 웜 캐시나 건너뛴 코드 경로가 가짜 속도 향상을 만들어낼 수 있습니다. 수치를 어디에든 올리기 전에 `/benchmark-checklist`를 실행하세요.
- **똑같은 실수를 수작업으로 반복 교정하기.** 채팅에서 교정하는 것은 그 한 번의 실행에만 도움이 됩니다. `/correct`는 이후 어떤 실행에서도 같은 실수가 반복되지 않도록 저장소를 고칩니다.
- **커버리지 확보 용도로 `/arena` 사용하기.** `/arena`는 하나의 설계나 코드 브리핑을 반복 시도한 뒤 베이스를 고르고 장점을 접목합니다. 슬라이스나 선언된 경합 부문을 나누어 맡고 하나의 보고서로 집계하는 작업에는 `/swarm`을 씁니다.
- **모든 리뷰 코멘트를 그대로 수용하기.** 봇과 사람 모두 진짜 지적과 단순 잡음을 한 목록에 섞어서 올립니다. `/interrogate`는 재실행으로 입증되는 발견 사항만 남기고 기각한 항목과 이유를 나열하며, 최종 결정은 여러분이 어느 쪽으로든 오버라이드할 수 있습니다.
- **`auto`를 모델 슬러그로 취급하기.** `auto`와 `inherit-parent`는 "서브에이전트가 부모 채팅 모델을 물려받도록 `model` 필드를 생략하라"는 뜻입니다. 역할 설정은 [설정 페이지](./01-setup.md)에서 다룹니다.
- **그린 빌드만 보고 성공 보고하기.** 빌드는 컴파일된다는 사실만 증명합니다. 실제 명령어, 플로우, 저장된 값 또는 프로파일을 요청하고 응답에 그 증거가 담기기를 기대하세요.
- **`SKILL.md`를 백지에서 수작업으로 작성하기.** 검증과 리뷰가 이루어지도록 [Authoring or modifying a skill 플레이북](../../skills/poteto-mode/playbooks/authoring-a-skill.md)을 통해 라우팅하세요.

가이드는 여기까지입니다. 중간을 건너뛰어 읽었다면 [설정 페이지](./01-setup.md)로 돌아가 실제 과업을 하나 실행해 보세요. 습관은 읽기가 아니라 사용을 통해 몸에 익습니다.

[가이드 목차](./README.md)로 돌아가기.

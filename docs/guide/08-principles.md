# 원칙 이름으로 방향 조율하기

pstack은 24개의 원칙을 개별 스킬로 제공합니다. `/poteto-mode`는 모든 다단계 과업의 시작 시점에 원칙 색인을 읽고, 과업이 트리거하는 원칙들을 적용하며, 응답에서 적용한 각 원칙의 이름과 그 원칙이 바꾼 의사결정을 명시합니다.

원칙은 슬래시 명령어로 직접 호출하는 대상이 아닙니다. 원칙의 이름을 불러 에이전트의 방향을 조율(steer)하는 것입니다. 각 이름은 에이전트가 이미 읽은 완전한 규칙 하나를 가리키므로, 지시 사항을 한 문단 길게 적는 것보다 짧은 구절 하나로 작업 방향을 훨씬 정밀하게 바로잡을 수 있습니다.

## 실전에서의 방향 조율

예를 들어 에이전트가 기존 어댑터 세 개 위에 새로운 어댑터를 또 덧붙이려 한다고 해봅시다:

```text
use subtract before you add. delete the obsolete adapters first, then design what's left.
```

빌드가 통과했다는 이유만으로 성공했다고 주장한다면:

```text
apply prove it works. run the real import flow and show me the written records.
```

두 개의 병렬 시도가 동일한 브랜치에 동시에 쓰기 작업을 하려 한다면:

```text
separate before serializing shared state. give each attempt its own worktree, no locks.
```

각 구절이 정확히 먹혀드는 이유는 그 뒤에 있는 규칙이 구체적이기 때문입니다. 또한 에이전트는 자신의 응답에서 그 규칙이 어떤 결정을 바꾸었는지 반드시 말해야 합니다. 바뀐 결정 없이 원칙 이름만 인용되어 있다면 에이전트가 규칙을 실제로는 적용하지 않고 이름만 읊었다는 뜻입니다.

## 24개 원칙 요약

핵심(core) 원칙들은 얼마나 만들지, 그리고 언제 설계를 처음부터 다시 생각할지를 결정합니다:

- [Laziness Protocol](../../skills/principle-laziness-protocol/SKILL.md): 삭제를 우선하고 문제를 해결하는 가장 작은 변경을 지향합니다.
- [Foundational Thinking](../../skills/principle-foundational-thinking/SKILL.md): 로직을 작성하기 전에 핵심 자료구조를 먼저 선택합니다.
- [Redesign from First Principles](../../skills/principle-redesign-from-first-principles/SKILL.md): 새로운 요구사항을 첫날부터 있었던 기초 가정처럼 통합하여 재설계합니다.
- [Attack the Premise](../../skills/principle-attack-the-premise/SKILL.md): 실패한 두 개 이상의 수정안이 공유하던 전제를 명시적으로 적어내고 다음 수정을 시도하기 전에 그 전제를 직접 테스트합니다.
- [Subtract Before You Add](../../skills/principle-subtract-before-you-add/SKILL.md): 무언가를 덧붙이기 전에 불필요한 군더더기(dead weight)를 먼저 제거합니다.
- [Minimize Reader Load](../../skills/principle-minimize-reader-load/SKILL.md): 읽는 이가 머릿속에 담아두어야 하는 계층과 숨겨진 상태를 허물어 줄입니다.
- [Outcome-Oriented Execution](../../skills/principle-outcome-oriented-execution/SKILL.md): 일회용 호환성 상태를 보존하는 대신 목표 설계로 재작성 작업을 수렴시킵니다.
- [Experience First](../../skills/principle-experience-first/SKILL.md): 구현 편의성보다 사용자가 얻는 결과를 우선합니다.
- [Exhaust the Design Space](../../skills/principle-exhaust-the-design-space/SKILL.md): 선례가 없을 때는 경쟁하는 프로토타입 두세 개를 직접 만들어 비교합니다.
- [Build the Lever](../../skills/principle-build-the-lever/SKILL.md): 리뷰어가 다시 실행해 볼 수 있도록 작업을 수행하거나 증명하는 스크립트를 만듭니다. 에이전트가 동일한 작업을 수작업으로 반복하고 있다면 스스로 아쉬워했을 도구나 스킬을 직접 작성하게 하세요. 스크립트가 매번 똑같은 방식으로 처리할 수 있는 단계라면 스크립트를 쓰고, 에이전트는 판단이 필요한 곳에 아껴 두세요.

아키텍처(architecture) 원칙들은 상태, 검증, 호환성이 어디에 위치해야 하는지를 결정합니다:

- [Model the Domain](../../skills/principle-model-the-domain/SKILL.md): 흩어진 조건문 대신 하나의 구조체로 반복되는 규칙을 인코딩합니다.
- [Boundary Discipline](../../skills/principle-boundary-discipline/SKILL.md): 경계에서 검증하고 내부 타입은 신뢰합니다.
- [Type System Discipline](../../skills/principle-type-system-discipline/SKILL.md): 잘못된 상태를 표현 불가능하게 만듭니다.
- [Make Operations Idempotent](../../skills/principle-make-operations-idempotent/SKILL.md): 재시도가 일어나도 항상 동일한 최종 상태로 수렴하게 만듭니다.
- [Migrate Callers Then Delete Legacy APIs](../../skills/principle-migrate-callers-then-delete-legacy-apis/SKILL.md): 한 번의 웨이브에서 호출자를 마이그레이션하고 레거시 API를 삭제합니다.
- [Separate Before Serializing Shared State](../../skills/principle-separate-before-serializing-shared-state/SKILL.md): 조율(락 등)을 추가하기 전에 상태 공유 자체를 먼저 제거합니다.

검증(verification) 원칙들은 무엇이 증거로 인정되는지를 정의합니다:

- [Prove It Works](../../skills/principle-prove-it-works/SKILL.md): 대리 지표(proxy)가 아니라 실제 아티팩트를 검증합니다.
- [Fix Root Causes](../../skills/principle-fix-root-causes/SKILL.md): 코드를 바꾸기 전에 재현하고 근본 원인까지 추적합니다.
- [Sequence Work into Verifiable Units](../../skills/principle-sequence-verifiable-units/SKILL.md): 다음 단위를 시작하기 전에 각 작은 작업 단위를 검증 가능한 상태로 마무리합니다.
- [Test Behavior, Not Implementation](../../skills/principle-test-behavior-not-implementation/SKILL.md): 코드의 사용자가 관찰할 수 있는 동작을 단언(assert)하여, 해당 동작이 깨질 때만 테스트가 실패하고 내부 구현 변경에는 깨지지 않게 합니다.
- [Explain the Number](../../skills/principle-explain-the-number/SKILL.md): 측정된 수치를 신뢰하거나 보고하기 전에 무엇이 그 수치를 제한하는지 밝히고 다른 것을 측정했을 가능성을 배제합니다. [`/benchmark-checklist`](../../skills/benchmark-checklist/SKILL.md)는 이를 실제 실행 결과로 답하는 일곱 가지 질문으로 바꾸어 줍니다.

위임(delegation) 원칙들은 병렬 작업을 질서 있게 유지합니다:

- [Guard the Context Window](../../skills/principle-guard-the-context-window/SKILL.md): 대량 읽기는 서브에이전트로 라우팅하고 메인 채팅에는 핵심 발견 사항만 유지합니다.
- [Never Block on the Human](../../skills/principle-never-block-on-the-human/SKILL.md): 되돌릴 수 있는(reversible) 작업은 멈추지 않고 먼저 진행한 뒤 결과를 제시합니다.

그리고 하나의 메타(meta) 원칙이 있습니다:

- [Encode Lessons in Structure](../../skills/principle-encode-lessons-in-structure/SKILL.md): 두 번 이상 반복해서 한 조언은 린트, 체크 또는 스크립트로 바꿉니다. [나만의 것으로 만들기](./09-make-it-yours.md#fix-the-environment-with-correct)에서 보여주듯 [`/correct`](../../skills/correct/SKILL.md)가 이를 저장소 전체에 적용합니다.

이 목록을 외우려 하지 마세요. 지금은 가볍게 훑어보고, 나중에 에이전트가 여기 있는 원칙 이름 하나로 막을 수 있었던 행동을 하는 순간 다시 돌아오세요. 어휘는 그렇게 몸에 익습니다.

다음: [나만의 것으로 만들기](./09-make-it-yours.md).

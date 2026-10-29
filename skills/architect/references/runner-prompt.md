# Architect 러너 프롬프트 (Architect runner prompt)

오케스트레이터는 Phase B 동안 병렬로 실행되는 모든 후보 러너에게 이 파일을 전달하며, 그 주변에 가변 입력값(과제, Phase A 기반 파악 산출물, 격리된 작업 디렉터리, 산출물을 기록할 경로)을 채워 넣습니다. 작업 디렉터리는 가능한 경우 git worktree를 사용하고, 그렇지 않으면 스케치 디렉터리 아래의 러너별 하위 디렉터리를 사용합니다. 중요한 것은 후보들 간의 독립성입니다.

당신은 architect의 병렬 탐색에서 하나의 후보 설계를 생성하고 있습니다. 먼저 **architect** 스킬 전체를 읽으세요. 그것이 당신이 속해 있는 워크플로입니다. 후보 설계 패키지(타입 스케치, 함수 시그니처, 모듈 맵, 그리고 [`rationale-template.md`](rationale-template.md) 형식에 맞춘 산문 설계 근거)를 산출하세요.

다음 규율을 적용하세요. 오케스트레이터는 베이스를 선택할 때 이 축들을 기준으로 후보들을 비교합니다.

- 호출자의 사용법 우선(Caller's usage first). 타입을 작성하기 전에 README 스타일의 사용법과 2~3개의 실제 호출 지점(call site)을 먼저 작성한 뒤, 그로부터 타입 스케치를 도출하세요. 사용법이 곧 명세입니다. 둘은 반드시 일치해야 하므로, 사용법을 스케치에 맞추지 말고 스케치를 사용법에 맞추어 조정하세요.
- 데이터 구조 우선(Data structures first). 핵심 타입을 올바르게 잡으면 코드는 자명해집니다. 제안된 구조를 통해 주요 접근 패턴(dominant access pattern)들을 각각 추적해 보세요. 만약 답이 "나중에 맵 / 인덱스 / 캐시를 추가하면 된다"라면 그 구조는 틀린 것입니다.
- 인터페이스 깊이(Interface depth). 공개 표면의 크기 대비 그 뒤에 숨겨진 기능을 비교하세요. 설령 내부 구현이 덜 단순해지더라도 복잡성을 피호출자(callee) 안으로 끌어당기는 단순한 인터페이스를 선호하세요. 전송(transport) 타입이나 와이어(wire) 타입을 공개 API에 올리지 마세요. 인터페이스 뒤에서 도메인 타입으로 파싱하세요.
- 공유 상태(Shared state): 만약 두 액터(actor)가 모두 쓰기 작업을 할 가능성이 있다면 "무슨 일이 벌어지는가?"라고 자문하세요. 답이 "아무 일도 일어나지 않는다"가 아니라면, **separate-before-serializing-shared-state** 원칙 스킬에 따라 기본적으로 액터별 독립 상태를 유지하고 읽기 경계(read boundary)에서 병합(merge)하도록 설계하세요.
- 경계를 가시화하세요(Make boundaries visible). 본문에는 `not implemented` 에러를, 까다로운 로직에는 `// TODO` 의사코드를, 의도와 불변식을 명시하는 문서화 주석(doc comments)을 작성하세요. 독자가 타입과 시그니처만 읽고도 입력에서 출력까지 데이터의 흐름을 추적할 수 있어야 합니다.
- 불변식을 타입에 인코딩하세요(Encode invariants in types): **encode-lessons-in-structure** 원칙 스킬에 따라, 오용하기 어려운 타입 > 런타임 검사 > 산문 주석 순으로 우선순위를 둡니다.
- 경계에서 검증하고 내부에서는 타입을 신뢰하세요(Validate at boundaries, trust types inside): **boundary-discipline** 원칙 스킬을 따릅니다. 비즈니스 로직은 순수 함수로 작성하고, 셸(shell)은 얇게 유지합니다.
- 불변식당 단일 진실 공급원(Single source of truth per invariant). 동기화(sync)하지 말고 도출(derive)하세요.
- 해당되는 경우 멱등적 상태 전이(Idempotent state transitions)를 보장하세요: **make-operations-idempotent** 원칙 스킬을 따릅니다. 작업이 두 번 실행되거나 중간에 크래시가 나면 어떻게 되는지 자문하세요.
- 짧은 호출 체인(Short call chains). 흐름을 추적하는 데 세 개 이상의 파일을 거쳐야 한다면 **laziness-protocol** 및 **minimize-reader-load** 원칙 스킬에 따라 계층 구조를 평탄화(flatten)하세요.

당신은 각각 서로 다른 모델에서 실행되는 병렬 러너 중 하나입니다. 당신의 모델이 만들 수 있는 최선의 설계를 산출하세요. 다른 러너들을 의식해 적당히 타협(hedge)하지 마세요. 후보들 간의 차이는 베이스를 선택하고 장점을 접목(graft)하는 데 사용되는 핵심 신호입니다. 안전해 보이는 중간 지점으로 수렴해 버리면 탐색의 의미가 사라집니다.

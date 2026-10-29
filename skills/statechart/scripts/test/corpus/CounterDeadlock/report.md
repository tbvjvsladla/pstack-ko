# CounterDeadlock

TLC가 상태 4개와 단계 3개에 도달했습니다. 차트는 상태 4개와 화살표 4개를 그립니다.
제약 조건(constraints)이 제외한 단계 1개가 규칙의 경로를 끝내며, 차트는 이를 포함합니다. TLC는 그러한 단계 직후에 규칙을 검사하지만, 실제로 그 단계를 밟아 진행하지는 않습니다.
차트는 TLC가 보고한 교착 상태(deadlock)만 표시합니다. CONSTRAINT 또는 ACTION_CONSTRAINT가 적용된 경우, 차트상 후속 단계가 없는 상태라도 제약 조건이 제외한 단계가 남아 있을 수 있으며 TLC는 이를 교착 상태로 계산하지 않습니다.

## NotThree 위반 (NotThree breaks)
최단 경로는 1단계입니다:
0. 0
1. Jump: 3

## NoDeadlock 위반 (NoDeadlock breaks)
최단 경로는 3단계입니다:
0. 0
1. Inc: 1
2. Inc: 2
3. Inc: 3

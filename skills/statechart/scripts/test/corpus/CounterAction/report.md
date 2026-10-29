# CounterAction

TLC가 상태 5개와 단계 4개에 도달했습니다. 차트는 상태 5개와 화살표 5개를 그립니다.
제약 조건(constraints)이 제외한 단계 1개가 규칙의 경로를 끝내며, 차트는 이를 포함합니다. TLC는 그러한 단계 직후에 규칙을 검사하지만, 실제로 그 단계를 밟아 진행하지는 않습니다.

## NotThree 위반 (NotThree breaks)
최단 경로는 1단계입니다:
0. 0
1. Jump: 3

## NotFour 위반 (NotFour breaks)
최단 경로는 4단계입니다:
0. 0
1. Inc: 1
2. Inc: 2
3. Inc: 3
4. Inc: 4

## 성립하는 규칙 (Rules that hold)
NoDeadlock

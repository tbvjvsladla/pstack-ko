# CounterTie

TLC가 상태 5개와 단계 4개에 도달했습니다. 차트는 상태 6개와 화살표 6개를 그립니다.
제약 조건(constraints) 밖의 상태 1개가 규칙의 경로를 끝내며, 차트는 이를 포함합니다. TLC는 제약 조건 밖의 상태에서도 규칙을 검사하지만, 그 상태를 넘어 탐색하지는 않습니다.
제약 조건이 제외한 단계 1개가 규칙의 경로를 끝내며, 차트는 이를 포함합니다. TLC는 그러한 단계 직후에 규칙을 검사하지만, 실제로 그 단계를 밟아 진행하지는 않습니다.

## NotThree 위반 (NotThree breaks)
최단 경로는 1단계입니다:
0. 0
1. Bump: 3

## NotOdd 위반 (NotOdd breaks)
최단 경로는 1단계입니다:
0. 0
1. Inc: 1

## NotFive 위반 (NotFive breaks)
최단 경로는 5단계입니다:
0. 0
1. Inc: 1
2. Inc: 2
3. Inc: 3
4. Inc: 4
5. Inc: 5

## 성립하는 규칙 (Rules that hold)
NoDeadlock

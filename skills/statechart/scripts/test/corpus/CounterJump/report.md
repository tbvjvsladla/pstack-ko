# CounterJump

TLC가 상태 6개와 단계 5개에 도달했습니다. 차트는 상태 7개와 화살표 6개를 그립니다.
제약 조건(constraints) 밖의 상태 1개가 규칙의 경로를 끝내며, 차트는 이를 포함합니다. TLC는 제약 조건 밖의 상태에서도 규칙을 검사하지만, 그 상태를 넘어 탐색하지는 않습니다.

## NotThree 위반 (NotThree breaks)
최단 경로는 1단계입니다:
0. jumps=0 x=0
1. Jump: jumps=1 x=3

## 성립하는 규칙 (Rules that hold)
NoDeadlock

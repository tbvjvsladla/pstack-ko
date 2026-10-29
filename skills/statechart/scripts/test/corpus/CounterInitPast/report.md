# CounterInitPast

TLC가 상태 0개와 단계 0개에 도달했습니다. 차트는 상태 1개와 화살표 0개를 그립니다.
모든 초기 상태가 제약 조건(constraints) 밖에 있으므로, TLC는 어떤 초기 상태도 탐색하지 않습니다.
제약 조건 밖의 상태 1개가 규칙의 경로를 끝내며, 차트는 이를 포함합니다. TLC는 제약 조건 밖의 상태에서도 규칙을 검사하지만, 그 상태를 넘어 탐색하지는 않습니다.

## NotNine 위반 (NotNine breaks)
최단 경로는 0단계입니다:
0. 9

## 성립하는 규칙 (Rules that hold)
NoDeadlock

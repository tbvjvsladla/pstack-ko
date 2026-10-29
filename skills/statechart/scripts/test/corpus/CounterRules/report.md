# CounterRules

TLC가 상태 2개와 단계 1개에 도달했습니다. 차트는 상태 4개와 화살표 3개를 그립니다.
제약 조건(constraints) 밖의 상태 2개가 각각 규칙의 경로를 끝내며, 차트는 이들을 포함합니다. TLC는 제약 조건 밖의 상태에서도 규칙을 검사하지만, 그 상태를 넘어 탐색하지는 않습니다.

## NotThree 위반 (NotThree breaks)
최단 경로는 1단계입니다:
0. 0
1. M!Jump: 3

## AtMostTwo 위반 (AtMostTwo breaks)
최단 경로는 1단계입니다:
0. 0
1. M!Jump: 3

## NotTwo 위반 (NotTwo breaks)
최단 경로는 2단계입니다:
0. 0
1. Step: 1
2. Step: 2

## 성립하는 규칙 (Rules that hold)
NoDeadlock

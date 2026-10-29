# UserNoDeadlock

TLC가 상태 4개와 단계 3개에 도달했습니다. 차트는 상태 4개와 화살표 3개를 그립니다.

## NoDeadlock 위반 (NoDeadlock breaks)
최단 경로는 1단계입니다:
0. 0
1. Next: 1

## CHECK_DEADLOCK 위반 (CHECK_DEADLOCK breaks)
최단 경로는 3단계입니다:
0. 0
1. Next: 1
2. Next: 2
3. Next: 3

# CounterFan

TLC가 상태 25개와 단계 36개에 도달했습니다. 차트는 상태 25개와 화살표 36개를 그립니다.

## NotDone 위반 (NotDone breaks)
최단 경로는 2단계입니다:
0. done=FALSE x=0
1. Pick: done=FALSE x=1
2. Finish: done=TRUE x=1

## 성립하는 규칙 (Rules that hold)
NoDeadlock

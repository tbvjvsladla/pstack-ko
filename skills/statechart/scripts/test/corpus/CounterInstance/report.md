# CounterInstance

TLC가 상태 3개와 단계 3개에 도달했습니다. 차트는 상태 3개와 화살표 3개를 그립니다.

## ReachesThree 위반 (ReachesThree breaks)
TLC의 반례(counterexample)는 tlc-ReachesThree.out에 있습니다. 반례는 3단계입니다:
0. 0
1. C!Up: 1
2. C!Up: 2
3. C!Back: 1
실행이 "1"과 "2" 사이를 무한히 반복(loop)합니다.

## 성립하는 규칙 (Rules that hold)
NoDeadlock

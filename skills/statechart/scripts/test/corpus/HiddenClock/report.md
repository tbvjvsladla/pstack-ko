# HiddenClock

TLC가 상태 2개와 단계 2개에 도달했습니다. 차트는 상태 2개와 화살표 1개를 그립니다.
차트는 동일한 차트 상태에 머무르는 1개의 단계는 그리지 않습니다.
VIEW가 적용된 경우, TLC는 동일하게 간주하는 상태 그룹마다 대표 상태(representative) 하나만 유지합니다. 화살표는 해당 단계가 실제로 도달하는 상태가 아니라 대표 상태로 연결될 수 있습니다.

## EndsAtZero 위반 (EndsAtZero breaks)
TLC의 반례(counterexample)는 tlc-EndsAtZero.out에 있습니다. 반례는 1단계입니다:
0. x=0 clock=0
1. Tick: x=1 clock=1
실행이 "x=1 clock=1"에서 영구히 멈춥니다.

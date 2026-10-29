# DieHard

TLC가 상태 16개와 단계 96개에 도달했습니다. 차트는 상태 16개와 화살표 58개를 그립니다.
차트는 동일한 차트 상태에 머무르는 38개의 단계는 그리지 않습니다.

## NotSolved 위반 (NotSolved breaks)
최단 경로는 6단계입니다:
0. big=0 small=0
1. FillBig: big=5 small=0
2. BigToSmall: big=2 small=3
3. EmptySmall: big=2 small=0
4. BigToSmall: big=0 small=2
5. FillBig: big=5 small=2
6. BigToSmall: big=4 small=3

## 성립하는 규칙 (Rules that hold)
TypeOK, NoDeadlock

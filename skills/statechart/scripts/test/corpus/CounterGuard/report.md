# CounterGuard

TLC가 상태 2개와 단계 1개에 도달했습니다. 차트는 상태 2개와 화살표 1개를 그립니다.
TLC가 차트 그리기 실행을 종료 코드 75(exit 75)로 중단했으므로, 차트는 중단되기 전에 도달한 상태들만 그립니다.

## TypeOK 위반 (TypeOK breaks)
TLC가 반례(counterexample)를 찾았습니다. 반례는 tlc-TypeOK.out에 있습니다.

## 검사하지 않은 규칙 (Rules not checked)
Lookup, NoDeadlock
TLC가 다음 규칙들을 검사하기 전에 종료 코드 75(exit 75)로 중단되었습니다:
```
Error: Evaluating invariant Lookup failed.
Attempted to apply tuple
<<"zero", "one">>
to integer 3 which is out of domain.
Error: The behavior up to this point is:
State 1: <Initial predicate>
x = 0

State 2: <Next line 6, col 9 to line 6, col 18 of module CounterGuard>
x = 1

State 3: <Next line 6, col 9 to line 6, col 18 of module CounterGuard>
x = 2

Error: The error occurred when TLC was evaluating the nested
expressions at the following positions:
0. Line 12, column 11 to line 12, column 30 in CounterGuard
1. Line 12, column 11 to line 12, column 21 in CounterGuard
```

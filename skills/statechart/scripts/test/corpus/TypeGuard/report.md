# TypeGuard

TLC가 상태 3개와 단계 2개에 도달했습니다. 차트는 상태 3개와 화살표 2개를 그립니다.
TLC가 차트 그리기 실행을 종료 코드 76(exit 76)으로 중단했으므로, 차트는 중단되기 전에 도달한 상태들만 그립니다.

## TypeOK 위반 (TypeOK breaks)
최단 경로는 2단계입니다:
0. 0
1. Next: 1
2. Next: 2

## 검사하지 않은 규칙 (Rules not checked)
NeedsTypeOK, NoDeadlock
TLC가 다음 규칙들을 검사하기 전에 종료 코드 76(exit 76)으로 중단되었습니다:
```
Error: Evaluating invariant NeedsTypeOK failed.
Attempted to apply tuple
<<"zero", "one">>
to integer 3 which is out of domain.
Error: The behavior up to this point is:
State 1: <Initial predicate>
x = 0

State 2: <Next line 6, col 9 to line 6, col 27 of module TypeGuard>
x = 1

State 3: <Next line 6, col 9 to line 6, col 27 of module TypeGuard>
x = 2
```

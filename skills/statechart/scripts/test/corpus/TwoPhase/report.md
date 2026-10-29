# TwoPhase

TLC가 상태 76개와 단계 192개에 도달했습니다. 차트는 상태 10개와 화살표 16개를 그립니다.
차트는 동일한 차트 상태에 머무르는 59개의 단계는 그리지 않습니다.
RMPrepare 단계는 한 번에 r1과 r2 중 하나만 변경합니다.
RMChooseToAbort 단계는 한 번에 r1과 r2 중 하나만 변경합니다.
RMRcvAbortMsg 단계는 한 번에 r1과 r2 중 하나만 변경합니다.
RMRcvCommitMsg 단계는 한 번에 r1과 r2 중 하나만 변경합니다.

## TCConsistent 위반 (TCConsistent breaks)
최단 경로는 5단계입니다:
0. Manager init, r1 working, r2 working
1. RMChooseToAbort: Manager init, r1 working, r2 aborted
2. RMPrepare: Manager init, r1 prepared, r2 aborted
3. TMRcvPrepared: Manager init, r1 prepared, r2 aborted
4. TMCommit: Manager done, r1 prepared, r2 aborted
5. RMRcvCommitMsg: Manager done, r1 committed, r2 aborted

## 성립하는 규칙 (Rules that hold)
TPTypeOK, NoDeadlock

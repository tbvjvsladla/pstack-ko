# TwoPhaseFixed

TLC가 상태 56개와 단계 142개에 도달했습니다. 차트는 상태 10개와 화살표 12개를 그립니다.
차트는 동일한 차트 상태에 머무르는 45개의 단계는 그리지 않습니다.
RMPrepare 단계는 한 번에 r1과 r2 중 하나만 변경합니다.
RMChooseToAbort 단계는 한 번에 r1과 r2 중 하나만 변경합니다.
RMRcvAbortMsg 단계는 한 번에 r1과 r2 중 하나만 변경합니다.
RMRcvCommitMsg 단계는 한 번에 r1과 r2 중 하나만 변경합니다.

## 성립하는 규칙 (Rules that hold)
TPTypeOK, TCConsistent, NoDeadlock

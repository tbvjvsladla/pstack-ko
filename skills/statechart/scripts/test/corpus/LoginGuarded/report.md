# LoginGuarded

TLC가 상태 8개와 단계 11개에 도달했습니다. 차트는 상태 7개와 화살표 10개를 그립니다.
ThePasswordIsRight 단계는 Session과 Tries를 동시에 변경합니다.
ThePasswordIsWrong 단계는 Session과 Tries를 동시에 변경합니다.

## 성립하는 규칙 (Rules that hold)
TypeOK, SignedInWhileLocked, NoDeadlock

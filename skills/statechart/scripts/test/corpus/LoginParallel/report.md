# LoginParallel

TLC가 상태 12개와 단계 29개에 도달했습니다. 차트는 상태 7개와 화살표 10개를 그립니다.
ThePasswordIsWrong 단계는 Session과 Tries를 동시에 변경합니다.
ThePasswordIsRight 단계는 Session과 Tries를 동시에 변경합니다.

## SignedInWhileLocked 위반 (SignedInWhileLocked breaks)
최단 경로는 5단계입니다:
0. Session Signed out, Tries No wrong tries
1. ThePasswordIsWrong: Session Signed out, Tries 1 wrong try
2. ThePasswordIsWrong: Session Signed out, Tries 2 wrong tries
3. ThePasswordIsWrong: Session Signed out, Tries Locked out
4. YouSubmitAPassword: Session Checking the password, Tries Locked out
5. ThePasswordIsRight: Session Signed in, Tries Locked out

## 성립하는 규칙 (Rules that hold)
TypeOK, NoDeadlock

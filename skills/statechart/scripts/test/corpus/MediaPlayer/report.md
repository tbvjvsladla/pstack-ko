# MediaPlayer

TLC가 상태 12개와 단계 64개에 도달했습니다. 차트는 상태 7개와 화살표 13개를 그립니다.
a call comes in 단계는 Playback과 Volume을 동시에 변경합니다.
the track ends 단계는 Playback과 Volume을 동시에 변경합니다.
you press stop 단계는 Playback과 Volume을 동시에 변경합니다.

## LoudWhenStopped 위반 (LoudWhenStopped breaks)
최단 경로는 1단계입니다:
0. Playback Stopped, Volume Normal volume
1. you turn it up: Playback Stopped, Volume Loud

## 성립하는 규칙 (Rules that hold)
TypeOK, NoDeadlock, EventuallyStops

# MediaPlayerProps

TLC가 상태 12개와 단계 64개에 도달했습니다. 차트는 상태 7개와 화살표 13개를 그립니다.
a call comes in 단계는 Playback과 Volume을 동시에 변경합니다.
the track ends 단계는 Playback과 Volume을 동시에 변경합니다.
you press stop 단계는 Playback과 Volume을 동시에 변경합니다.

## EventuallyPlays 위반 (EventuallyPlays breaks)
TLC의 반례(counterexample)는 tlc-EventuallyPlays.out에 있습니다. 반례는 0단계입니다:
0. Playback Stopped, Volume Normal volume
실행이 "Playback Stopped, Volume Normal volume"에서 영구히 멈춥니다.

## QuietWhenStopped 위반 (QuietWhenStopped breaks)
TLC의 반례(counterexample)는 tlc-QuietWhenStopped.out에 있습니다. 반례는 1단계입니다:
0. Playback Stopped, Volume Normal volume
1. you turn it up: Playback Stopped, Volume Loud

## 성립하는 규칙 (Rules that hold)
TypeOK, NoDeadlock, EventuallyStops, StaysTyped

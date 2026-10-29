---
name: principle-laziness-protocol
description: >-
  리팩터링하거나, diff 크기를 평가하거나, 추상화·계층·시그널 전달(threading)을 추가하려는 유혹을 느낄 때
  적용합니다. 삭제와 문제 해결을 위한 최소한의 변경 쪽으로 편향되게 행동하세요.
disable-model-invocation: true
---

# 게으름 프로토콜 (Laziness Protocol)

- **의사결정을 통합하세요.** 동일한 선택을 여러 곳에서 반복하지 마세요. 단일 진실 공급원(single source of truth) 뒤에 두고 그 결과를 단순한 플래그로 전달하세요.
- **diff를 최소화하세요.** 문제를 해결하는 가장 작은 변경을 만드세요. "우아한" 보일러플레이트보다 줄 수가 적은 것이 낫습니다.

# Counter

TLC가 상태 4개와 단계 3개에 도달했습니다. 차트는 상태 4개와 화살표 3개를 그립니다.

## 성립하는 규칙 (Rules that hold)
Low, StaysSmall
Low에는 [], <>, ~>가 없으므로 TLC는 초기 상태에서만 이를 검사했습니다. 모든 상태를 검사하려면 INVARIANT로 등록하세요.

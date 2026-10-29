### 성능 이슈 (Perf issue)

**측정 스토리(measurement story)에 대한 소유권은 당신에게 있습니다. 계획하고, 리뷰하고, 수치를 검증하세요.** 모든 수정을 측정값에 연결하고, 측정하는 대신 소스를 읽는 것으로 대체하지 마세요.

1. 해당 표면의 제어(control) 스킬을 통해 베이스라인 트레이스를 캡처하세요. 베이스라인과 이후의 모든 수치를 **benchmark-checklist** 스킬로 검증하세요.
2. 가설의 근거를 다지기 위해 `how`를 실행하세요. 직접 실행해 보지 않고 성능 상한선(perf ceiling)을 주장하지 마세요.
   가장 저렴한 것부터 순서대로 성능 만트라(performance mantras)를 시도하세요:
   1. 하지 마라 (Don't do it). 아무 데서도 결과를 쓰지 않는 작업은 비용을 줄이려 하지 말고 아예 중단하세요.
   2. 하되, 다시 하지는 마라 (Do it, but don't do it again).
   3. 더 적게 하라 (Do it less).
   4. 나중에 하라 (Do it later).
   5. 아무도 안 볼 때 하라 (Do it when they're not looking).
   6. 동시에 하라 (Do it concurrently).
   7. 더 저렴하게 하라 (Do it cheaper).

   앞선 만트라가 목표치를 충족하면 거기서 멈추세요.
3. 트레이스로부터 수정을 계획하세요. 함수 경계를 넘어선다면 먼저 `architect`를 실행하세요. 설정된 perf-issue 모델(기본값 `grok-4.7-xhigh-fast`)을 사용하는 서브에이전트에 구현을 위임하세요. 서브에이전트(Subagents) 섹션에 따라 작업에 맞게 각 서브에이전트의 추론 수준과 빠른(fast) 또는 표준(standard) 속도를 선택하세요. diff를 리뷰하세요. 수정 후(post-fix) 트레이스를 캡처하세요.
   **sequence-verifiable-units** 원칙 스킬을 적용하여 다음 시도를 하기 전에 각 시도를 검증하세요.
4. 산출물을 파싱하고 비교하세요(JSON을 sqlite로 변환, diff 비교). "결론 내릴 수 없음(Inconclusive)"이나 다른 표면에서의 확인은 통과가 아닙니다. 이를 명확히 표시하세요.
5. PR에 측정값을 인용하세요.
6. **Opening a PR**을 실행하세요.

일회성 수정이 아니라 지표에 대한 지속적인 개선이 목적이라면 Hillclimb 플레이북(`playbooks/hillclimb.md`)을 사용하세요.

**응답(Reply):** 베이스라인 수치, 수정 후 수치, 델타(변화량), 산출물 경로.

# 라우팅 맵 예시 (Routing map example)

이 파일을 `.cursor/automations/benny/` 바깥(예: `.cursor/benny/routing.md`)으로 복사하고 모든 플레이스홀더를 실제 값으로 교체하세요. `routing.map_path`가 그 복사본을 가리키게 하세요. 팩을 새로고침하더라도 이 파일이 덮어써져서는 안 됩니다.

트리아지 스킬은 이 파일을 데이터로 취급합니다. 라우팅에는 제보 내용이나 원인 추적(cause trace)에서 나온 증거가 필요합니다. 단순 키워드 일치만으로는 충분하지 않습니다.

```yaml
routes:
  - name: "billing-example"
    match:
      product_areas:
        - "billing-area-placeholder"
      code_paths:
        - "billing-code-path-placeholder"
      error_signatures:
        - "billing-error-placeholder"
    destination:
      slack_channel: "billing-channel-placeholder"
      tracker_team: "billing-team-placeholder"
    owners:
      - "billing-owner-placeholder"
    allow_feature_owner_ping: false

  - name: "desktop-example"
    match:
      product_areas:
        - "desktop-area-placeholder"
      code_paths:
        - "desktop-code-path-placeholder"
      error_signatures:
        - "desktop-error-placeholder"
    destination:
      slack_channel: "desktop-channel-placeholder"
      tracker_team: "desktop-team-placeholder"
    owners:
      - "desktop-owner-placeholder"
    allow_feature_owner_ping: false

fallback:
  destination: ""
  owners: []
  allow_feature_owner_ping: false

ping_policy:
  default: "off"
  allow:
    - "configured-feature-owner"
    - "confirmed-regression-author"
  deny:
    - "broad-on-call-group"
    - "unverified-owner"
```

## 규칙

- 하나의 팀이 매칭되지 않은 모든 제보를 전담해서 받는 경우가 아니라면 `fallback.destination`은 비워 두세요.
- 안정적인 제품 영역(product areas), 코드 경로(code paths), 에러 시그니처(error signatures)를 사용하세요.
- 공개되는 사본에는 비공개 데이터를 포함하지 마세요.
- 게시될 예시 파일에 날것의 사용자 ID나 채널 ID를 붙여넣지 마세요.
- 대상 팀이 동의하기 전까지는 기능 오너 호출(feature-owner ping)을 꺼 두세요.
- 리라우트(reroute)는 제보자에게 어디로 가야 하는지 알려줄 뿐입니다. 자동화가 직접 다른 채널에 교차 게시(cross-post)하는 일은 절대 없습니다.

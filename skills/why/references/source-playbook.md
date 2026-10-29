# 소스 플레이북 (Source playbooks)

`why` 스킬은 사용 가능한 근거 카테고리마다 한 명의 조사자를 생성(spawn)하며, 각 조사자는 아래의 소스별 플레이북 중 하나를 읽습니다. 이 플레이북들은 흔히 쓰이는 MCP들에 대한 구체적인 예시입니다. 동일한 카테고리의 다른 MCP를 사용하는 경우 그에 맞게 변형하여 적용하세요.

| 카테고리 | 플레이북 | 문서화된 예시 MCP |
|---|---|---|
| 소스 컨트롤 히스토리 (Source control history) | [`code-archaeology.md`](./sources/code-archaeology.md) | git, `gh` |
| 이슈 / 티켓 트래커 (Issue / ticket tracker) | [`linear.md`](./sources/linear.md) | Linear (Jira, GitHub Issues, Plane, Shortcut에 맞게 변형) |
| 장문 문서 (Long-form documents) | [`notion.md`](./sources/notion.md) | Notion (Confluence, Google Docs, Coda에 맞게 변형) |
| 실시간 팀 채팅 (Real-time team chat) | [`slack.md`](./sources/slack.md) | Slack (Discord, Microsoft Teams, Mattermost에 맞게 변형) |
| 인프라 옵저버빌리티 (Infrastructure observability) | [`datadog.md`](./sources/datadog.md) | Datadog (New Relic, Honeycomb, Grafana, Splunk에 맞게 변형) |
| 에러 / 예외 트래킹 (Error / exception tracking) | [`sentry.md`](./sources/sentry.md) | Sentry (Rollbar, Bugsnag, Airbrake에 맞게 변형) |
| 제품 분석 웨어하우스 (Product analytics warehouse) | [`databricks.md`](./sources/databricks.md) | Databricks SQL (Snowflake, BigQuery, ClickHouse, dbt에 맞게 변형) |

교차 영역 (Cross-cutting):

- [`incident-postmortem.md`](./sources/incident-postmortem.md). 대상 코드가 방어적 코드(null 체크, 재시도, 타임아웃, 속도 제한, 피처 플래그, 이그레스 가드, OOM 핸들러)로 보일 경우 이 플레이북을 함께 추가하세요.

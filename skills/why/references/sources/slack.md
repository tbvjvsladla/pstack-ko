# Slack 대화 (Slack Conversations)

## 검색 방법 (How to search it)

Slack MCP 도구는 환경마다 다릅니다. 어떤 Slack MCP를 사용할 수 있는지 확인하고 먼저 도구 스키마를 점검하세요. `mcp_auth`가 필요할 수 있습니다. 인증에 실패하면 즉시 중단하고 공백(gap)으로 보고하세요.

1. **작성자 기반 검색 (Author-bounded search).** PR 머지 날짜 전후로 PR 작성자가 보낸 메시지. 검색 범위를 극적으로 줄여주며 핵심 정보를 바로 찾는 경우가 많습니다.
3. **PR URL 검색.** Slack에서는 PR을 리뷰하거나 논의할 때 링크를 공유하는 경우가 많습니다. PR URL(또는 단순히 `/pull/<number>`)을 검색하세요.
4. **에러 문자열 검색.** 코드가 특정 에러를 처리한다면 해당 에러 문자열을 검색하세요. 인시던트 스레드가 발굴되는 경우가 많습니다.

## 흔한 함정 (Common pitfalls)

- **채널 고고학의 한계 (Channel archaeology limits).** 아주 오래된 메시지는 보존 정책(retention policies) 때문에 사라졌을 수 있습니다. 특정 날짜 이전의 메시지를 전혀 찾을 수 없다면 보존 기간 절벽(retention cliff)으로 기록하세요.
- **검색 불가능한 DM (Unsearched DMs).** 많은 의사결정이 검색할 수 없는 DM에서 이루어집니다. 이는 놓칠 수밖에 없으며, 알려진 한계점입니다.
- **"의사결정"처럼 보이는 농담이나 추측 (Speculative jokes as "decisions").** Slack은 캐주얼합니다. 커밋 직전에 나왔더라도 "ㅋㅋ 그냥 그렇게 해버려(Lol just do the thing)"는 의사결정이 아닙니다. 신중하게 논의된 대화를 찾으세요.
- **단일 메시지의 맥락 붕괴 (Context collapse in single messages).** 스레드 전체를 보지 않으면 단일 메시지는 원래 맥락과 다르게 읽히는 경우가 많습니다. 항상 스레드 전체를 가져오세요.
- **인증 실패 (Auth failures).** MCP가 인증되지 않았다면 멈추세요. 조사 결과를 지어내지 마세요. Slack을 검색할 수 없었다고 보고하세요.

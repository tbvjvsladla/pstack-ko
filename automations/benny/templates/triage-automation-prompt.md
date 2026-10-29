# 트리아지 자동화 프롬프트 (Triage automation prompt)

> 복사된 셋업 워크플로우를 위한 원본 자료입니다. 복사된 팩이 자동화가 실행될 저장소에 커밋되었음을 `automate`가 확인한 후, 내장 `automate` 초안에 이 의도를 패러프레이즈하여 반영하세요.

이번 실행에서는 `.cursor/automations/benny/skills/triage-issue-reports/SKILL.md`를 읽고 따르세요.

설정 소스입니다. 이 저장소 상대 경로는 동일한 대상 저장소에 커밋된 경우에만 포함하세요. 그렇지 않다면 설정된 값들을 풀어서 설명(paraphrase)하세요. 플러그인 원본 경로와 캐시 경로는 절대 사용하지 마세요:

```text
{{BENNY_CONFIG_PATH}}
```

트리거:

```json
{
	"source_channel_id": "{{SLACK_CHANNEL_ID}}",
	"message_ts": "{{SLACK_MESSAGE_TS}}",
	"thread_ts": "{{SLACK_THREAD_TS_OR_EMPTY}}"
}
```

생성 의도(creation intent)는 이를 설정된 소스 Slack 채널의 새로운 최상위 제보로 기술해야 합니다.

소스 채널과 루트 스레드 타임스탬프를 불변(immutable)으로 취급하세요. 둘 중 하나라도 누락되었거나 설정과 일치하지 않으면 게시하거나 이슈 트래커에 기록하지 말고 즉시 중단하세요.

커밋된 운영 파일이 분류, 첨부 파일 검토, 원인 추적, 라우팅, 중복 제거(dedupe), 트래커 기록 및 최종 판정을 소유합니다. 진행 상황 메시지는 게시하지 마세요. 소스 채널에 루트 메시지를 게시하는 일은 절대 없어야 합니다.

코디네이터만 유일하게 Slack에 게시할 수 있습니다. 위임받은 모든 워커는 읽기 전용이어야 하며, 발견 사항만 반환하고, 모든 Slack 쓰기 동작에 대한 명시적인 금지 지시를 받아야 합니다.

단일 판정 메시지의 끝에는 설정된 마커 중 정확히 하나만 붙이세요:

```text
[benny:bug]
[benny:performance]
[benny:other]
```

버그 또는 성능 마커에는 `tracker=<URL>`을 덧붙일 수 있습니다.

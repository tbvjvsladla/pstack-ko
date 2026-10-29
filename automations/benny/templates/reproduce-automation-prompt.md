# 재현 자동화 프롬프트 (Reproduce automation prompt)

> 복사된 셋업 워크플로우를 위한 원본 자료입니다. 복사된 팩이 자동화가 실행될 저장소에 커밋되었음을 `automate`가 확인한 후, 내장 `automate` 초안에 이 의도를 패러프레이즈하여 반영하세요.

이번 실행에서는 `.cursor/automations/benny/skills/reproduce-and-fix-issues/SKILL.md`를 읽고 따르세요.

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

생성 의도(creation intent)는 이를 설정된 소스 Slack 채널의 새로운 최상위 제보로 기술해야 합니다. 또한 설정된 저장소, 기본 브랜치, 이슈 트래커, 컨트롤 어댑터, 기능 맵, 그리고 드래프트 풀 리퀘스트 생성 권한을 포함해야 합니다.

소스 채널과 루트 스레드 타임스탬프를 불변(immutable)으로 취급하세요. 둘 중 하나라도 누락되었거나 설정과 일치하지 않으면 아무것도 게시하지 말고 즉시 중단하세요.

이 정확한 스레드에서 설정된 트리아지 ID가 남긴 설정된 트리아지 마커를 기다리세요. 오직 `[benny:bug]` 또는 `[benny:performance]`인 경우에만 진행하세요.

재현을 시도하기 전에 설정된 컨트롤 어댑터 스킬이 반드시 있어야 합니다. 실제 UI를 통해 구별 가능한 정확한 증상을 두 번 재현하세요. 기존 풀 리퀘스트나 커밋은 덮어쓰지 말고 검증만 수행하세요. 확인된 재현과 운영 파일의 수정 게이트(fix gate)를 통과한 후에만 범위가 제한된 수정을 시도하세요.

코디네이터만 유일하게 Slack에 게시할 수 있습니다. 모든 하위(child) 프롬프트는 `SendSlackMessage`, `PostToSlack`, `chat.postMessage` 및 그 밖의 모든 Slack 쓰기 작업을 금지해야 합니다. 하위 에이전트는 발견 사항만 반환합니다.

소스 채널에 루트 메시지를 게시하는 일은 절대 없어야 합니다.

# benny

benny는 Slack 이슈 제보를 처리하는 두 개의 Cursor 자동화(automations)를 제공합니다. 하나는 각 제보를 트리아지하고, 다른 하나는 확인된 버그를 재현하며 작은 드래프트 수정안을 준비할 수 있습니다.

이 디렉터리의 파일들은 비활성 상태의 셋업 및 자동화 원본입니다. 슬래시 스킬로는 나타나지 않습니다.

## 설정 방법

1. Cursor에 [`FOR_AGENTS.md`](./FOR_AGENTS.md)를 가리키고 대상 저장소 이름을 알려줍니다.
2. 셋업 과정이 이 디렉터리 전체를 대상 저장소의 `.cursor/automations/benny/`에 병합하도록 합니다. 이때 대상 저장소에만 있는 파일은 보존하고, 로컬 편집 내용을 덮어쓰는 대신 충돌을 검토해야 합니다.
3. 공유 의존성을 사용할 수 있도록 셋업 과정이 대상 저장소의 `.cursor/settings.json`에서 pstack을 활성화하도록 합니다:

```json
{
	"plugins": {
		"pstack": { "enabled": true }
	}
}
```

4. 사용자 소유의 설정은 복사된 팩 바깥(예: `.cursor/benny/`)에 보관합니다. [`configuration.example.yaml`](./templates/configuration.example.yaml)과 [`feature-map.example.md`](./skills/reproduce-and-fix-issues/references/feature-map.example.md)을 맞게 수정하여 사용하세요.
5. 어느 한쪽 자동화라도 활성화하기 전에 `.cursor/settings.json`, `.cursor/automations/benny/`, 그리고 시크릿이 없는 모든 설정 파일을 커밋합니다.
6. 에디터에서 각 신규 자동화 초안을 검토하거나 기존 자동화를 업데이트합니다. 그런 다음 무해한 테스트 제보를 보내 모든 소스 채널 게시물이 원본 스레드 안에만 머무는지 확인합니다.

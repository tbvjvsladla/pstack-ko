# benny 자동화 의도 (Automation Intent)

## 자동화하려는 대상

하나의 Slack 이슈 채널에서 함께 작동하는 두 개의 Cursor 자동화(automations)를 원합니다.

### 자동화 1: 이슈 제보 트리아지 (triage issue reports)

- 트리거: 설정된 소스 Slack 채널에 누군가 새로운 최상위(top-level) 제보를 올리면, 이 자동화가 해당 제보에서 시작되어 원래의 스레드 좌표를 유지하기를 원합니다.
- 동작: 스레드와 첨부 파일을 읽고, 제보를 버그 또는 성능 이슈, 기능 요청, 질문 또는 피드백, 혹은 리라우트(reroute)로 분류한 뒤 라우팅 전에 가능성이 높은 소유 계층(owning layer)을 추적하기를 원합니다.
- 트래커: 설정된 트래커에서 중복 이슈를 검색하고, 확실한 중복 건은 업데이트하며, 명확한 신규 버그에 대해서만 티켓을 생성하기를 원합니다.
- 도구: Slack 스레드 읽기 및 답글 권한, 설정된 트래커 연동, 그리고 선택적인 라우팅 맵을 사용하기를 원합니다.
- 결과: 소스 스레드에 짧은 판정 결과 및 `[benny:bug]`, `[benny:performance]`, 또는 `[benny:other]`가 포함된 답글을 정확히 하나만 남기기를 원합니다. 버그 또는 성능 마커에는 트래커 URL이 포함될 수 있습니다.
- 경계: 이 자동화가 소스 채널에 루트(root) 메시지를 게시하는 일은 절대 없기를 원합니다.

### 자동화 2: 확인된 버그 재현 및 수정 (reproduce and fix confirmed bugs)

- 트리거: 이 자동화가 동일한 신규 최상위 제보(또는 셋업 중 선택한 다른 지원 트리거)에서 시작된 후, 원본 스레드에 신뢰할 수 있는 트리아지 마커가 올라올 때까지 대기하기를 원합니다.
- 게이트: 누군가 수정을 명확히 소유하고 있다면 멈추기를 원합니다. 기존 풀 리퀘스트나 머지된 커밋이 해당 제보를 해결할 가능성이 있다면 경쟁하는 변경을 만드는 대신 검증을 수행하기를 원합니다.
- 동작: 설정된 컨트롤 어댑터와 기능 맵(feature map)을 사용하여 실제 UI를 통해 정확한 증상을 두 번 재현하고, 스크린샷, 비디오 및 읽기 전용 상태 교차 검증(cross-check) 결과를 캡처하기를 원합니다.
- 수정: 기존 풀 리퀘스트를 덮어쓰지 않고 검증하기를 원합니다. 재현이 확인된 후에는 범위가 제한된(bounded) 근본 원인 수정을 1회 시도할 수 있으며, 테스트 비용이 저렴할 때는 TDD를 사용하고, 영향 반경(blast radius)을 스모크 테스트한 뒤, 전후(before-and-after) 증명이 통과할 때만 드래프트 풀 리퀘스트를 열기를 원합니다.
- 도구: Slack 스레드 읽기 및 답글 권한, 저장소 및 히스토리 접근 권한, 드래프트 풀 리퀘스트 생성 권한, 설정된 트래커, 그리고 컨트롤 어댑터를 사용하기를 원합니다.
- 결과: 소스 스레드 또는 선택적 운영(operations) 스레드에 증거와 검증된 결과를 남기고, 선택적으로 드래프트 풀 리퀘스트를 추가하기를 원합니다. 업데이트는 간결해야 합니다.
- 경계: 이 자동화가 소스 채널에 루트 메시지를 게시하는 일은 절대 없기를 원합니다.

### 공통 규칙

- 전체 실행 동안 소스 채널과 루트 스레드 좌표는 불변(immutable)으로 유지되어야 합니다.
- 유틸리티 봇과 디버그 봇은 위임이나 수정 소유권이 아니라 증거로 취급합니다.
- 서브에이전트가 돕는 것은 허용하지만, 서브에이전트가 Slack에 게시하거나 Slack 자격 증명을 전달받는 것은 금지합니다.
- 이 팩 전체가 대상 저장소의 `.cursor/automations/benny/`에 커밋되기를 원합니다. 내부의 `SKILL.md` 파일들은 등록된 플러그인 스킬이 아니라 직접적인 자동화 지침입니다.
- `how`, `why`, `tdd`, `unslop` 및 필수 원칙 스킬들과 같은 공유 의존성을 위해서만 대상 저장소의 커밋된 `.cursor/settings.json`을 통해 pstack이 활성화되기를 원합니다.
- 각 라이브 자동화 프롬프트가 커밋된 운영 파일을 직접 읽기를 원합니다. 플러그인 캐시 경로, 복사된 발췌문, 또는 슬래시 스킬 탐색에 의존하는 것을 원하지 않습니다.
- 팩 새로고침 시 덮어써지지 않도록 사용자 소유 설정, 기능 맵, 라우팅 맵 및 시크릿은 `.cursor/automations/benny/` 바깥에 보관합니다.
- 채널 좌표, 트래커 접근 권한, 컨트롤 어댑터 또는 기능 맵이 누락되었거나 불확실할 때 두 자동화 모두 안전하게 실패 후 종료(fail closed)하기를 원합니다.
- 오직 드래프트 풀 리퀘스트만 원합니다. 머지하거나 배포하지 마세요.

### 내 설정

- 소스 Slack 채널: `<channel>`
- 선택적 운영(operations) 채널: `<channel or none>`
- 저장소 및 기본 브랜치: `<repo>`, `<branch>`
- 트래커: `<type, team, project, labels, intake status>`
- 라우팅 맵: `<path or none>`
- 트리아지 ID(identity): `<slack identity>`
- 컨트롤 스킬: `<configured skill or adapter>`
- 기능 맵(feature map): `<committed same-repo path outside the copied pack, or behavior to paraphrase>`
- 모델: `<triage, reproduce, code, media review>`
- 상태 이모지 문자열: `<seen, reproducing, reproduced, blocked, fixing, failed, pull request opened>`
- 예산(budgets): `<polling, verdict wait, follow-up, repro, rejection, fix>`
- 선택적 봇 토큰 기능: `<none, file download, or editable operations status>`

[`configuration.example.yaml`](./templates/configuration.example.yaml)과 [`feature-map.example.md`](./skills/reproduce-and-fix-issues/references/feature-map.example.md)에서 시작하세요. 이 팩 바깥(예: `.cursor/benny/` 아래)에 복사하여 내용을 채우세요. 시크릿 값은 시크릿 매니저나 환경 변수에 보관하세요.

## 에이전트용 지침

사람은 Cursor에 이 파일을 가리키는 것으로 셋업을 시작합니다. 탐색된 benny 슬래시 스킬을 찾거나 호출하지 마세요.

1. 어느 저장소에서 자동화를 실행할지 물어봅니다.
2. 이 `FOR_AGENTS.md`가 들어 있는 디렉터리를 소스 팩으로 취급합니다.
3. 전체 소스 팩을 `<target-repository>/.cursor/automations/benny/`로 병합(merge)합니다.
4. 대상 저장소에만 있는 파일은 모두 보존합니다. 관련 없는 파일을 삭제하거나 사용자 소유의 설정, 기능 맵, 라우팅 맵을 절대 덮어쓰지 마세요.
5. 소스가 관리하는 경로에 있는 기존 대상 파일의 내용이 다를 경우, diff를 검토하고 로컬 편집 내용을 버리지 않으면서 병합합니다. 소유권이 모호하면 교체하기 전에 멈추고 물어봅니다.
6. 복사된 `FOR_AGENTS.md`와 `skills/setup-benny/SKILL.md`가 대상 저장소에 존재하는지 확인합니다.
7. 대상 저장소에서 직접 `.cursor/automations/benny/skills/setup-benny/SKILL.md`를 읽고 따릅니다.

대상 저장소의 `.cursor/settings.json`에 다음 항목을 병합하세요:

```json
{
	"plugins": {
		"pstack": { "enabled": true }
	}
}
```

관련 없는 모든 설정과 플러그인을 보존하세요. 기존 파일이 JSONC를 사용하는 경우 주석과 유효한 JSONC 문법을 보존하세요.

대상 저장소를 루트로 하는 새로운 에이전트로부터 검증을 받기를 원합니다. pstack의 `how`, `why`, `tdd`, `unslop` 및 benny가 사용하는 원칙 스킬들이 프로젝트 범위(project scope)에서 해석(resolve)되는지 확인하세요. 현재 세션이나 사용자 범위(user-scoped) 설치에서 로드된 스킬은 세지 마세요.

프로젝트 범위 플러그인을 사용할 수 없거나 공유 의존성 중 하나라도 해석되지 않으면 멈추고 무엇이 실패했는지 설명하세요. `.cursor/automations/benny/skills/`를 플러그인 매니페스트에 추가하거나 그 파일들이 슬래시 스킬 목록에 나타나기를 기대하지 마세요.

어느 한쪽 자동화라도 활성화하기 전에 `.cursor/settings.json`, `.cursor/automations/benny/`, 그리고 참조된 시크릿 없는 설정 파일들이 반드시 커밋되어야 한다고 안내하세요. 제가 명시적으로 요청하기 전까지는 자동화를 생성하거나 업데이트하지 마세요.

최초 생성 시에는 트리아지용으로 내장 `/automate`를 한 번 사용하고, 재현 및 수정용으로 한 번 사용하세요. 첫 번째 자동화에 대한 초안 검토, 승인, 준비 상태 점검 및 Automations 에디터 인계를 모두 완료한 뒤에 두 번째 자동화를 시작하세요.

이 의도와 완성된 설정을 각 초안에 맞게 패러프레이즈(paraphrase)하여 반영하세요. 트리아지 프롬프트는 `.cursor/automations/benny/skills/triage-issue-reports/SKILL.md`를 읽고 따라야 합니다. 재현 프롬프트는 `.cursor/automations/benny/skills/reproduce-and-fix-issues/SKILL.md`를 읽고 따라야 합니다. 이러한 저장소 상대 경로는 자동화가 실행될 저장소에 해당 파일들이 커밋되었음을 `/automate`가 확인한 후에만 사용하세요.

기존 자동화의 경우 이를 검사하거나 업데이트하기 위해 `/automate`를 사용하지 마세요. 설정을 검증한 뒤, 제가 에디터에서 각 자동화를 직접 수정할 수 있도록 복사된 셋업 파일의 간결한 필드 체크리스트를 사용하세요. 중복 자동화를 만들지 마세요.

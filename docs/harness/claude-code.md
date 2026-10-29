# Claude Code에서의 pstack (pstack in Claude Code)

pstack 스킬들은 Cursor의 명칭을 사용합니다. Claude Code에서는 세션당 한 번 이 파일을 읽은 뒤, 아래 표를 사용해 스킬 내부의 각 Cursor 명칭을 매핑하세요. `<pstack>`은 `skills/`와 `docs/`가 들어 있는 폴더를 의미합니다. Cursor 필드에 해당하는 행이 표에 없다면 해당 필드를 생략하고 그 사실을 언급하세요. 도구 스키마가 거부하는 필드나 값은 절대 전달하지 마세요.

## 도구 (Tools)

| Cursor | Claude Code |
| --- | --- |
| `Task` | `Agent` |
| `AskQuestion` | `AskUserQuestion`. `-p` 모드에서는 이 도구가 없으므로 일반 텍스트로 질문하세요. |
| 할 일 목록(todolist) 열기 | 세션에서 제공하는 경우 `TodoWrite`를 사용하세요. 그렇지 않으면 응답 본문에 목록을 유지하고 각 단계마다 업데이트하세요. |
| 다른 pstack 스킬 실행 (`/name` 또는 "the **name** skill") | `<pstack>/skills/<name>/SKILL.md`를 `Read`로 읽고 따르세요. `Skill` 도구는 대부분의 pstack 스킬을 거부합니다. 사용자에게 직접 입력하라고 안내할 때는 `/pstack:<name>`으로 표기하세요. |
| `Grep`, `Glob` | 세션에 `Grep`이나 `Glob`이 없을 때는 `rg`를 사용하는 `Bash`로 대체하세요. |

## 서브에이전트 필드 (Subagent fields)

| Cursor | Claude Code |
| --- | --- |
| `subagent_type: generalPurpose` | `subagent_type: "general-purpose"` |
| `poteto-agent`, `comment-sicko` | `pstack:poteto-agent`, `pstack:comment-sicko` |
| `readonly: true` | `subagent_type: "Explore"`. `Bash` 도구가 그대로 유지되므로 브리핑에 "파일을 편집하지 마세요(do not edit files)"라고 명시하고, 작업 후 `git status`를 확인하세요. |
| `readonly: false` | `general-purpose`. 세션의 MCP 서버들을 그대로 유지합니다. |
| `environment: "cloud"` | `isolation: "worktree"`. 워커가 체크아웃할 브랜치나 커밋을 명시하세요. |
| `environment: "local"` | 생략합니다. |
| `cloud_base_branch: <b>` | 워커에게 먼저 `<b>`를 체크아웃하라고 지시하세요. |
| `run_in_background: true` | 생략합니다. 서브에이전트는 기본적으로 백그라운드에서 실행되며 완료 시 알림을 보냅니다. |
| `resume` | 새 에이전트를 스폰하고 이전 결과를 브리핑에 붙여넣으세요. |
| `file_attachments` | 브리핑에 파일 경로들을 나열하세요. |

## 모델 (Models)

| Cursor | Claude Code |
| --- | --- |
| `claude-opus-5-5-xhigh`와 같은 `claude-<family>-<version>-<effort>[-fast]` 형식 | `model: "opus"` 및 `effort: "xhigh"`. `model`은 오직 `opus`, `sonnet`, `haiku`, `fable`만 받습니다. `-fast`는 제거하세요. |
| `grok-*`, `gpt-*` 또는 기타 비-Claude 슬러그 | 에이전트가 세션 모델에서 실행되도록 `model`을 생략하고 그 사실을 언급하세요. |
| `auto` 또는 `inherit-parent` | `model`을 생략합니다. |
| `pstack-models.mdc` 규칙 (`~/.cursor/rules/pstack-models.mdc`) | `~/.claude/rules/pstack-models.md`. Claude Code는 모든 새 세션에 이 파일을 로드하므로 역할 줄들이 이미 컨텍스트에 들어 있습니다. 들어 있지 않다면 파일을 직접 읽으세요. 파일이나 해당 역할 줄이 없으면 스킬의 기본 슬러그를 사용하세요. |
| `opus high`와 같은 `<model> <effort>` 형식의 역할 값 | `model: "opus"` 및 `effort: "high"`. |

## setup-pstack

Claude Code는 모든 새 세션마다 `~/.claude/rules/` 안의 모든 `.md` 파일을 로드하므로 `setup-pstack`은 그곳에 규칙을 작성합니다. 다음 변경 사항을 적용하여 단계를 따르세요:

- 감지(Detect). 감지된 모델은 `Agent` 도구 스키마가 나열하는 `model` 값들이며, 각각 스키마의 `effort` 값 중 하나와 짝을 이룹니다. 실제 모델은 `opus xhigh`처럼 `<model> <effort>` 형식으로 작성하세요. effort가 마지막 토큰이 되므로 예산(budget) 단계가 원문 그대로 작동합니다.
- 기본값(Defaults). 위의 표를 사용해 각 기본 슬러그를 매핑합니다: `claude-opus-5-5-xhigh`는 `opus xhigh`가 되고, `grok-4.7-xhigh-fast`는 `inherit-parent`가 됩니다.
- 작성(Write). `~/.claude/rules/pstack-models.md`에 frontmatter 없이 주석 줄들, `# budget` 줄, 그리고 5단계의 역할별 줄을 한 줄씩 작성합니다. 파일 전체를 덮어쓰세요. 역할 줄들을 `CLAUDE.md`나 `AGENTS.md`에 절대 넣지 마세요.

## 경로 (Paths)

| Cursor | Claude Code |
| --- | --- |
| `.cursor/skills/`, `~/.cursor/skills/` | `.claude/skills/`, `~/.claude/skills/` |
| `~/.cursor/plugins/`, `.cursor-plugin/plugin.json` | `~/.claude/plugins/`, `.claude-plugin/plugin.json` |
| `.cursor/rules/*.mdc`, 상시 적용(always-applied) 규칙 | `CLAUDE.md` (또는 `CLAUDE.md`가 없을 때는 `AGENTS.md`). Claude Code는 `.mdc` 파일을 절대 읽지 않습니다. 모델 규칙만 예외입니다(위의 Models 참조). |
| `https://github.com/cursor/plugins/blob/main/pstack/<path>` | 해당 파일이 존재할 때는 `<pstack>/<path>`. 플러그인은 스킬들과 `docs/guide/`, `README.md`를 함께 배포합니다. 원격에서 가져오지 말고 로컬 사본을 읽으세요. 사용자에게 링크가 필요할 때만 URL을 안내하세요. |
| `~/.cursor/projects/<slug>/agent-transcripts/` 안의 트랜스크립트 | `~/.claude/projects/<slug>/<session>.jsonl`. `<slug>`는 워크스페이스 경로의 모든 `/`와 `.`을 `-`로 바꾼 값입니다(예: `/Users/you/proj` → `-Users-you-proj`). 해당 폴더 바로 아래에 있는 가장 최신 `.jsonl` 파일이 현재 세션입니다. `<session>/subagents/` 폴더에 서브에이전트 트랜스크립트가 들어 있습니다. 현재 워크스페이스의 폴더만 읽으세요. |

## Cursor 전용 기능 (Cursor-only features)

Claude Code에는 Cursor Automations, Bugbot, Cursor 클라우드 에이전트, 내장 PR 도구가 없습니다. 특정 단계에서 이 기능이 필요할 때는 응답에 `Skipped in Claude Code: <step>. <reason>.` 한 줄을 적고 다음 단계로 넘어가세요. 멈추거나, 물어보거나, 재시도하지 마세요. 이후 단계에서 건너뛴 단계가 만든 결과물(URL이나 키 등)이 필요한 경우 플레이스홀더를 남겨 두고 해당 안내 줄에 그 이름을 명시하세요.

| Cursor | Claude Code |
| --- | --- |
| Cursor Automations: 루틴(routines), `update_state` 루틴 호출, `api2.cursor.sh/automations` 웹훅, 루틴 깨우기(wakes), `SendToUser` 시크릿 요청 | 안내 한 줄을 적고 건너뜁니다. |
| Bugbot | GitHub PR에 달린 Bugbot 코멘트는 일반 리뷰 스레드와 같으므로 해당 단계가 제시하는 포지(forge) 명령어로 트리아지하세요. PR에 Bugbot 검사나 코멘트가 없을 때 Bugbot을 시작하거나 기다리거나 설정하는 단계는 건너뜁니다. |
| Cursor 클라우드 에이전트 | `environment: "cloud"`는 서브에이전트 필드 표에 따라 워크트리(`isolation: "worktree"`)로 매핑됩니다. 대시보드 상태나 고유 VM 머신처럼 클라우드 에이전트 자체가 필요한 단계는 건너뜁니다. |
| 내장 PR 도구 | `gh pr create`처럼 해당 단계가 제시하는 포지 명령어를 사용하세요. 이는 건너뛰기(skip)가 아니므로 별도의 안내 줄을 적지 마세요. |

---
name: reflect
description: >-
  활성 트랜스크립트에 대해 세 개의 병렬 리뷰 서브에이전트를 실행하여 배운 점(learnings)을 도출하고, 각각을 기존
  스킬에 대한 구체적인 수정 사항으로 라우팅합니다. 사용자가 reflect라고 말할 때 사용하세요.
disable-model-invocation: true
---

# Reflect

현재 대화에서 오래 유지될 배운 점(durable learnings)을 발굴한 뒤, 이를 스킬 수정 사항으로 라우팅합니다.

사용자가 "reflect" 또는 "/reflect"라고 말할 때 호출하세요. 대화가 사소하거나, 주제에서 벗어났거나, 부모 에이전트가 올바르게 따랐던 기존 스킬에 이미 다뤄진 내용이라면 건너뛰세요. 일회성 사건은 배운 점이 아닙니다.

## 1. 활성 트랜스크립트 찾기

팬아웃하기 전에 시스템 프롬프트가 지정한 `agent-transcripts/` 디렉터리에서 현재 대화의 트랜스크립트를 찾으세요. `~/.cursor/projects/*/` 전체에 대해 글로빙(glob)하지 마세요. 그렇게 하면 워크스페이스 경계를 넘어 관련 없는 프로젝트의 비공개 채팅을 읽게 됩니다.

```bash
ls -t <agent-transcripts>/*.jsonl <agent-transcripts>/*/*.jsonl <agent-transcripts>/*/subagents/*.jsonl 2>/dev/null | head -10
```

이 명령은 기존의 평면 구조(`<id>.jsonl`), 현재의 중첩 구조(`<id>/<id>.jsonl`), 서브에이전트 구조(`<parent>/subagents/<child>.jsonl`) 레이아웃을 모두 포괄합니다. 각 후보 파일에서 첫 번째 JSONL 줄을 읽으세요. `message.content[0].text`에 현재 대화의 첫 사용자 프롬프트가 포함된 파일을 선택합니다. 일치하는 파일이 없으면 세션의 간결한 요약본(digest)을 대신 전달하세요.

## 2. 세 명의 리뷰어를 병렬로 실행하기

하나의 메시지에서 세 번의 `Task` 호출을 `subagent_type: generalPurpose`, 에이전트 모드(`readonly: false`)로 실행합니다. 리뷰어들은 트랜스크립트가 참조하는 티켓, 채팅 스레드, 옵저버빌리티 트레이스를 조회하기 위해 MCP 접근 권한이 필요하며, readonly 모드는 MCP를 제거합니다.

각 리뷰어와 통합자(synthesizer)는 `pstack-models.mdc` 규칙의 역할 줄(role line)과 기본값을 지정합니다. `model`을 해당 줄의 값으로 설정하거나, 규칙이나 해당 줄이 없으면 기본값으로 설정하세요. 값이 `auto` 또는 `inherit-parent`인 경우 `model`을 설정하지 않은 채로 두세요. Task 도구가 슬러그를 거부하면 기본값을 사용하고 그 사실을 알리세요. 기본값도 거부하면 에러 메시지에서 동일한 모델 패밀리의 가장 가까운 유효한 슬러그를 사용하세요. **poteto-mode** 스킬의 Subagents 섹션에 따라 작업에 맞는 각 서브에이전트의 추론 수준과 속도(빠름 또는 표준)를 선택하세요.

| 렌즈 | 역할 줄 (Role line) | 기본 `model` | 프롬프트 템플릿 |
|---|---|---|---|
| Judgment (판단) | `reflect judgment, divergent, synthesizer` | `claude-opus-5-5-xhigh` | `references/judgment-reviewer.md` |
| Tooling (도구) | `reflect tooling` | `grok-4.7-xhigh-fast` | `references/tooling-reviewer.md` |
| Divergent (발산) | `reflect judgment, divergent, synthesizer` | `claude-opus-5-5-xhigh` | `references/divergent-reviewer.md` |

표시된 위치에 트랜스크립트 경로 또는 요약본을 대입하여 각 템플릿을 원문 그대로 전달하세요. 리뷰어들은 `Task` 응답 본문에 발견 사항(findings)을 담아 반환합니다.

## 3. 통합하기 (Synthesize)

`subagent_type: generalPurpose`, `reflect judgment, divergent, synthesizer` 줄의 `model`(기본값 `claude-opus-5-5-xhigh`), 에이전트 모드(`readonly: false`)로 한 번의 `Task` 호출을 실행합니다. 통합자는 MCP를 통해 인용을 표본 검증하며, readonly 모드는 MCP를 제거합니다. 표시된 위치에 각 리뷰어의 전체 출력을 인라인으로 삽입하여 `references/synthesizer.md`를 원문 그대로 전달하세요. 통합자는 승인(Accepted) / 거부(Rejected) / 백로그(Backlog) 목록을 반환합니다.

## 4. 구조적 강제 여부 확인 (Structural enforcement check)

린트 규칙, 스크립트, 메타데이터 플래그 또는 런타임 검사로 더 확실하게 강제할 수 있는 Accepted 항목은 모두 Backlog로 이동시키세요. **encode-lessons-in-structure** 원칙 스킬을 참조하세요.

## 5. 적용하기 (Apply)

통합자의 전체 Accepted / Rejected / Backlog 출력을 제시하고, Accepted 수정 사항을 적용하기 전에 명시적인 승인을 기다리세요. 사용자가 적용할 부분집합을 선택하며 라우팅을 변경할 수도 있습니다. 스킬 변경은 조직 내 향후 모든 에이전트에게 영향을 미칩니다. 자동으로 적용하지 마세요.

각 Backlog 항목은 기다리지 말고 팀의 devex 또는 백로그 트래커에 바로 등록하세요. Accepted 목록만 승인을 기다립니다.

절대 `plugins/cache/` 아래의 스킬을 직접 수정하지 마세요. 승인 전에 대상 경로인 `.cursor/skills/<skill>/` 또는 `~/.cursor/skills/<skill>/`을 보여주세요. 승인 후에는 스킬 폴더를 해당 위치로 복사한 뒤 복사본을 수정하세요. 절대 PR을 열거나 푸시하지 마세요.

승인된 각 행의 Routing을 정확히 따르세요:

- 사소한 기존 스킬 수정(한 줄짜리 불릿 추가, 문장 다듬기, 오래된 사실 수정): 부모 에이전트가 직접 수행합니다.
- 실질적인 기존 스킬 수정(새 섹션, 새 패턴 표, 약 10줄 이상의 변경): Cursor의 내장 `create-skill` 스킬에 넘겨 초안 작성 / 테스트 / 반복 루프를 실행하게 하세요.
- `tune description: <skill path>`(스킬이 존재하지만 트리거되어야 할 때 작동하지 않은 경우): `create-skill`에 넘겨 설명 최적화 루프를 실행하게 하세요.
- `new skill via create-skill: <kebab-name>`: 생성 작업을 `create-skill`에 넘기세요. 임의로 형태를 지어내지 마세요.

환경에 SKILL.md 검증기(validator)가 포함되어 있다면 완료를 선언하기 전에 수정한 모든 스킬에 대해 실행하세요.

## 6. 사용자를 위한 요약

서두 없이 짧은 목록으로 작성합니다:

- 적용된 수정 사항: `<skill path>`. 무엇이 바뀌었는지 각각 한 줄로 설명.
- 생성된 새 스킬: `<skill path>`. 각각 한 줄로 설명 (드묾).
- devex 트래커에 등록된 백로그: `<issue title>` (`<tags>`). 각각 한 줄로 설명.
- 제외됨(Dropped): 거부된 발견 사항당 한 줄 + 통합자가 제시한 사유.

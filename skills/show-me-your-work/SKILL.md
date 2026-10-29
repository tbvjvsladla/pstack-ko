---
name: show-me-your-work
description: >-
  장시간 실행되거나 무인으로 진행되는 작업을 위해 검토 가능한 의사결정 기록을 유지합니다: 결정당 한 행(무엇을, 왜,
  근거, 결과)으로 구성된 TSV 로그입니다. 기본적으로 로컬에 유지하되, 리뷰어가 결과를 신뢰하기 위해 기록이 필요할 때는
  커밋하세요. /show-me-your-work, 자율 실행 또는 다단계 실행, 혹은 사람이 자리를 비웠다가 돌아와서
  검토하는 작업에 사용하세요.
disable-model-invocation: true
---

# Show me your work

단 하나의 정본(canonical) 로그를 유지하세요.

## 형식

단일 TSV 파일이며, 의사결정당 한 행입니다. 각 셀은 한 줄로 유지합니다. 근거(evidence)는 산문이 아니라 포인터입니다.

깨끗한 로그를 시작하려면 `references/decision-log-template.tsv`(헤더 행)를 복사하세요. 열 구성:

- **ts.** ISO8601 타임스탬프.
- **phase.** 단계 또는 작업 스트림(workstream).
- **decision.** 선택하거나 수행한 내용, 한 줄.
- **why.** 평이한 말로 적은 이유. 특정 원칙이 기반이 되었다면 전문 용어 태그가 아니라 평이한 말로 풀어서 적으세요.
- **evidence.** 이를 증명하는 링크 또는 경로: 커밋 SHA, PR 번호, `file:line`, 또는 아티팩트, 트레이스, 스크린샷 경로. 절대 문단으로 적지 마세요.
- **result.** 결과 또는 조건(predicate) 상태: `tests green`, `reverted`, `pixel-diff 0`, `INCONCLUSIVE`, `open`.

리뷰어가 한눈에 읽을 수 있도록 평이한 말로 작성한 예시입니다.

```
ts	phase	decision	why	evidence	result
2026-05-24T09:02:00Z	frame	counted the work first, about 100 components and roughly 75 hours	wanted to know the size before starting a long run	commit 3a9f1c2	found 5 things to sort out before starting
2026-05-24T09:40:00Z	harness	took screenshots of the old version before changing anything	so we can compare old against new and catch any visual change	scripts/snapshot.sh, baseline/	saved 120 reference screenshots
2026-05-24T11:15:00Z	widget	moved the widget styles over without changing how it looks	keep the change small and the result identical	commit 7c21e0a, pixel-diff 0	looks identical, tests pass
2026-05-24T12:30:00Z	widget	threw out a helper's work because its screenshots were blank	checked the real files instead of trusting its summary	worktree reset	reverted, tightened the instructions for next time
```

## 행 기록하기

각 항목은 팀원에게 자신이 한 일을 말해주듯 작성하세요. 평이한 단어와 구체적인 행동을 사용하고, AI 특유의 말투나 추상적인 전문 용어는 쓰지 마세요(**unslop** 스킬은 로그 텍스트에도 동일하게 적용됩니다).

헬퍼 스크립트인 `scripts/log.sh <logfile> <phase> <decision> <why> <evidence> <result>`를 사용하세요. 이 스크립트는 `ts`를 찍고, 첫 사용 시 헤더를 작성하며, 불필요한 탭/줄바꿈을 제거하고, `=`, `+`, `-`, `@`로 시작하는 셀 앞에 작은따옴표를 붙입니다. 단순 `printf`로 행을 추가해도 되지만, 셀 내용이 생성된 텍스트나 사용자 입력에서 온다면 동일한 바이트 처리에 주의하세요.

모든 사소한 행동이 아니라 의사결정 지점과 체크포인트를 기록하세요: 선택한 갈래(fork), 검증 결과와 함께 완료된 작업 단위, 트리거와 함께 발생한 방향 전환(pivot)이나 되돌리기(revert), 드러난 블로커(blocker), 수정된 게이트. 루프 실행의 경우 반복(iteration)당 한 행을 기록합니다. 사소하고 자명한 것은 건너뛰세요.

하나의 실행(run)은 이후 턴들과 그 요약본을 포함한 단일 에이전트 대화입니다. 작업 이어받기(pickup), 교체 에이전트, 또는 새 채팅은 새로운 실행을 시작합니다. 이미 행이 있는 로그에 실행이 내용을 추가할 때, 그 첫 번째 행은 phase로 `start`를 가지며, 다른 실행의 `start` 행 다음에 오는 첫 번째 행도 마찬가지입니다. 따라서 이후 턴에 로그로 다시 돌아온 실행은 먼저 로그의 마지막 행들을 읽어 그사이에 다른 실행이 기록을 남겼는지 확인합니다. `start` 행은 자기 앞에 있는 행들 중 이번 실행이 작성하지 않은 행들의 `ts` 범위를 명시하며, 그 `evidence`에는 에이전트 ID처럼 이번 실행을 식별하는 이름을 적습니다. 다른 용도로는 절대 `start` 단계를 사용하지 마세요.

## 저장 위치

기본적으로 로그는 커밋하지 않는 작업 산출물입니다. 작업 디렉터리의 `decisions.tsv`에 두거나, 여러 작업이 동시에 실행될 때는 `.audit/<task-slug>.tsv`에 두고 git 추적에서는 제외하세요.

작업의 규모가 충분히 야심 차서 리뷰어가 결과를 신뢰하기 위해 반드시 의사결정 기록이 필요한 경우에만 커밋하세요.

## 규칙

- 추가 전용(Append-only). 잘못된 판단이 있었다면 이를 대체(supersede)하는 새 행을 추가합니다. 과거 기록을 절대 수정하거나 삭제하지 마세요.
- 수작업으로 만든 일회성 근거보다 커밋된 스크립트가 생성한 근거를 선호하세요(**encode-lessons-in-structure** 원칙 스킬).

## 트랜스크립트와 대조하여 로그 감사하기

실행이 끝날 때 결과를 넘기기 전에 로그가 진실을 말했는지 확인하세요. 활성 워크스페이스의 `agent-transcripts/` 디렉터리(시스템 프롬프트가 경로를 지정함) 아래에서 이번 실행의 트랜스크립트를 읽으세요. `~/.cursor/projects/*/` 전체를 글로빙하지 마세요. 관련 없는 비공개 채팅을 읽게 됩니다. 실제로 일어난 일과 이번 실행의 행들을 대조해 보세요. 이번 실행에 해당하는 각 구간은 이번 실행의 `start` 행 중 하나(또는 이번 실행이 로그를 처음 만들었다면 첫 번째 행)에서 시작하여 다른 실행의 다음 `start` 행에서 끝납니다:

- 모든 행이 실제 의사결정이나 실제 행동에 매핑되는지 확인하세요.
- 각 행의 근거(evidence)가 실제로 확인 가능하며 행이 주장하는 바를 보여주는지 확인하세요.
- 작업의 향방을 결정지었지만 로그에 기록되지 않은 갈래, 방향 전환, 폐기된 접근법은 누락(gap)입니다. 추가하세요.

이야기를 고치지 말고 로그를 교정하세요. 감사 과정에서는 지어낸 행이라 할지라도 절대 기존 행을 편집하거나 삭제하지 않습니다. 어떤 행이 실제 의사결정도 실제 행동도 기록하지 않았거나 그 주장이나 근거가 틀렸다면, 실제로 일어난 일과 확인 가능한 포인터를 담아 기존 행을 대체하는 새 행을 추가하세요. 이 감사는 이번 실행의 구간 밖에 있는 행들은 검사하지 않습니다. 다만 이번 실행의 작업 과정에서 이전 행 중 하나가 틀렸음이 드러난다면 다른 잘못된 판단과 마찬가지로 대체 행을 추가하세요.

## 기록에 대한 교차 모델 리뷰 (Cross-model review)

결과를 넘기기 전에 작업을 수행한 모델과 다른 모델 패밀리의 서브에이전트를 띄우세요. 셀프 리뷰는 이를 대체할 수 없습니다. 서브에이전트는 감사 기록(audit trail)과 실행 트랜스크립트를 읽은 뒤, 사용자가 주의 깊게 봐야 할 부분을 표시합니다. 작업을 다시 수행하는 것이 아니라 차선책이었거나 위험한 부분을 스캔하는 것입니다. **poteto-mode** 스킬의 Subagents 섹션에 따라 작업에 맞는 각 서브에이전트의 추론 수준과 속도(빠름 또는 표준)를 선택하세요.

- 빈약하거나 누락된 근거로 기록된 결정.
- 트랜스크립트에 증명 없이 건너뛰었거나 완료했다고 주장한 검증 단계.
- 사후에 보았을 때 위험해 보이는 선택(성급한 결정, 범위 확장(scope-creeping), 증상만 임시방편으로 덮어버린 조치).
- 사용자가 가볍게 훑어볼 때 놓치기 쉬운 공백.

기록을 생성한 실행의 모든 응답은 마지막에 "Attention" 섹션으로 끝납니다. 첫 줄에 리뷰어의 모델을 단독 줄(`reviewed by <model>`)로 적고, 특정 행이나 순간을 가리키는 각 지적 사항(flag)을 나열하세요. "No flags"는 유효한 값이지만 모델 이름은 생략할 수 없습니다.

## 기록 검토하기

위에서 아래로 읽으며 근거 포인터를 따라가 표본 검사를 수행하세요. GitHub는 커밋된 TSV를 표로 렌더링합니다. 터미널에서는 `column -s$'\t' -t decisions.tsv`로 표 형태를 볼 수 있습니다.

## 이 스킬 조합하기 (Composing)

다른 스킬들은 자체적인 감사 기록 형식을 새로 만들지 않고 이곳으로 라우팅합니다. 이름으로 이 스킬을 참조하고 형식은 이 스킬이 소유하도록 두세요. 열 구성을 중복해서 다시 설명하지 마세요.

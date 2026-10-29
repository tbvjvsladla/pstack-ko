---
name: setup-pstack
description: >-
  pstack이 역할별로 어떤 모델을 어떤 추론 예산(reasoning budget)으로 사용할지 설정합니다. 사용 가능한
  모델을 감지하고 스킬 기본값을 재정의하는 상시 적용(always-applied) 규칙을 작성합니다.
  /setup-pstack, 'pstack 모델 설정(configure pstack models)', 'pstack
  예산(pstack budget)' 또는 pstack의 모델 선택을 변경할 때 사용하세요.
---

# Setup pstack

역할별 pstack의 모델을 설정하는 상시 적용 규칙인 `~/.cursor/rules/pstack-models.mdc`를 작성합니다.

## 단계

### 1. 사용 가능한 모델 감지

이번 세션에서 `Task` 서브에이전트에 전달할 수 있는 모델 슬러그들을 나열하세요. 그것이 신뢰할 수 있는 출처입니다. 만약 Cursor가 사용자에게 권한이 부여된 모델을 나열하는 모델 API나 CLI도 노출한다면 완전성을 위해 그것을 우선하세요. 아무것도 감지할 수 없다면 사용자에게 접근 가능한 슬러그를 붙여넣어 달라고 요청하세요. 사용 가능하다고 확인되지 않은 실제 슬러그는 절대 기록하지 마세요. 별칭인 `inherit-parent`와 `auto`는 감지된 슬러그가 아니더라도 언제나 유효합니다.

### 2. 현재 상태 불러오기

기본 역할-모델 매핑은 아래 5단계에 표시된 규칙 형태입니다. `~/.cursor/rules/pstack-models.mdc`가 이미 존재한다면 파일을 읽어 그 안의 `# budget` 줄과 역할 값들을 현재 선택 사항으로 취급하세요. 존재하지 않으면 기본값에서 시작합니다. `how critics`처럼 5단계에 없는 역할을 가진 줄은 폐기된(retired) 역할에서 온 것이므로 제거하세요.

### 3. 예산 설정, 매핑 및 확인

**(a) 예산을 물어보세요.** 자유 텍스트보다 AskQuestion을 우선 사용하세요. 아래의 정확한 라벨로 네 가지 옵션을 제시하고, 규칙에 현재 예산이 기록되어 있다면 함께 알려주세요. 규칙이 없다면 `large`가 스킬 기본값과 일치한다고 안내하세요.

- `unlimited — max reasoning`
- `large — xhigh reasoning`
- `medium — high reasoning`
- `small — medium reasoning`

**(b) 예산을 적용하세요.** 스킬 기본값으로부터 작업 표를 만들고, 재실행 시에는 패밀리, 목록 또는 별칭(`inherit-parent`, `auto`)으로 변경했던 역할을 유지하세요. `unlimited`, `large`, `medium`, `small`은 패널 항목을 포함한 모든 실제 슬러그의 추론 강도(effort) 토큰을 각각 `max`, `xhigh`, `high`, `medium`으로 설정합니다. 추론 강도 토큰은 마지막 토큰이거나 끝에 붙는 `fast` 바로 앞의 토큰이며, 단계는 `max` > `xhigh` > `high` > `medium` > `low` 순입니다. 변환 결과가 감지된 슬러그에 없다면, 동일한 패밀리의 감지된 슬러그 중 목표 강도 이하에서 가장 높은 강도를 가진 슬러그를 사용하고, 그것도 없으면 해당 역할을 선택 필요(needing a choice)로 표시하세요. `inherit-parent`와 `auto`는 변하지 않습니다. 따라서 `unlimited`는 `claude-opus-5-5-xhigh`를 `claude-opus-5-5-max`로 바꿉니다. Grok 슬러그는 `xhigh`가 최대치이므로, `unlimited`에서는 폴백 규칙에 따라 Grok을 `xhigh`로 두어 `grok-4.7-xhigh-fast`를 그대로 유지합니다. `large`는 두 기본값을 모두 유지합니다. `small`은 이들을 각각 `claude-opus-5-5-medium`과 `grok-4.7-medium-fast`로 바꿉니다.

**(c) 역할 목록을 보여주고 확인받으세요.** 모든 역할과 그 모델을 보여주되, 감지된 집합에 없는 실제 슬러그는 선택이 필요한 항목으로 표시하세요. 또한 2단계에서 제거한 줄도 모두 나열하세요. 그대로 수락할지 특정 역할을 변경할지 물어보며, 감지된 모델들과 함께 `inherit-parent` 및 `auto`(둘 다 이 역할이 부모 채팅 모델에서 실행됨을 의미하며, Auto 사용자가 Auto 상태를 유지하는 방법임)를 옵션으로 제시하세요. 자유 텍스트보다 AskQuestion을 우선 사용하세요. 패널 역할(arena runners, architect runners, interrogate reviewers)의 경우 값은 목록 형태이며, 별칭 항목을 포함해 항목당 하나의 서브에이전트가 실행되므로 목록의 길이가 곧 실행 개수를 결정합니다. `arena cross-judge pool`도 목록이지만 Arena는 한 명의 심사위원(judge)만 실행합니다. 부모 모델과 다른 패밀리에 속한 값들만 목록 순서대로 시도하므로 선호도 순으로 정렬하세요. `inherit-parent`나 `auto` 값은 절대 심사위원이 되지 않습니다. `swarm workers`는 레이스(race)나 비교 작업에서 갈래(arm)별로 다른 모델을 할당하지 않는 한 모든 워커의 기본 모델이 됩니다.

### 4. 검증

기록되는 모든 실제 슬러그는 반드시 감지된 집합 안에 있어야 합니다. `inherit-parent`와 `auto`는 항상 통과합니다. 선택된 실제 슬러그를 사용할 수 없다면 멈추고 다시 물어보세요.

### 5. 규칙 작성

`alwaysApply: true`, 선택한 라벨과 목표 추론 강도가 적힌 `# budget` 줄, 그리고 poteto-mode가 사용하는 것과 동일한 라벨을 사용한 역할별 한 줄씩을 담아 `~/.cursor/rules/pstack-models.mdc`를 작성하세요. 재실행 시 멱등성(idempotent)이 유지되도록 파일 전체를 덮어쓰세요. 형태는 다음과 같습니다:

```
---
description: pstack per-role model choices (overrides skill defaults)
alwaysApply: true
---
# pstack model configuration. One line per role. Delete a line to fall back to the skill default.
# `inherit-parent` or `auto` as a value: the role runs on the parent chat model (omit Task `model`). Alias entries in a panel list still count toward its fan-out.
# budget: large (xhigh)
feature, refactoring: grok-4.7-xhigh-fast
bug-fix: grok-4.7-xhigh-fast
perf-issue: grok-4.7-xhigh-fast
hillclimb: grok-4.7-xhigh-fast
judgment and prose: claude-opus-5-5-xhigh
hardest tasks: claude-opus-5-5-xhigh
how explorer: grok-4.7-xhigh-fast
how explainer: claude-opus-5-5-xhigh
why investigators: grok-4.7-xhigh-fast
why synthesizer: claude-opus-5-5-xhigh
reflect tooling: grok-4.7-xhigh-fast
reflect judgment, divergent, synthesizer: claude-opus-5-5-xhigh
arena runners: claude-opus-5-5-xhigh, grok-4.7-xhigh-fast
arena cross-judge pool: claude-opus-5-5-xhigh, grok-4.7-xhigh-fast
swarm workers: grok-4.7-xhigh-fast
architect runners: claude-opus-5-5-xhigh, grok-4.7-xhigh-fast
interrogate reviewers: claude-opus-5-5-xhigh, grok-4.7-xhigh-fast
```

### 6. 확인 안내

사용자에게 규칙이 작성되었으며 새 세션부터 적용된다고 알려주세요. 이 스킬을 다시 실행하면 규칙이 업데이트됩니다.

### 7. 검증 스킬 제안 (선택 사항)

프로젝트에 실제 앱을 구동하여 증명을 수행하는 방법(`verify-*` 스킬 또는 기존 하네스)이 있는지 확인하세요. 없다면 한 번 제안하세요: "에이전트가 사용자처럼 앱을 구동하고 변경 사항이 작동함을 증명할 수 있도록 프로젝트 로컬 검증 스킬을 만드시겠어요? `/create-verification-skill`로 생성해 드릴 수 있습니다." 예(yes)라고 답하면 `/create-verification-skill`을 호출하세요(워크스페이스, 사용자 또는 플러그인 중 pstack이 설치된 위치에서 확인됨). 아니오(no)라고 답하면 강요하지 말고 넘어가세요.

### 8. 정형 검증 스킬 제안 (선택 사항)

프로젝트에 이미 정형 검증(formal verification) 스킬(코드에 대해 모델 체커, 증명기 또는 기타 검증기를 실행하는 프로젝트 스킬)이 있는지 확인하세요. 없다면 한 번 제안하세요: "테스트가 닿는 경우뿐만 아니라 범위 내의 모든 경우에서 코드의 핵심 속성이 성립함을 에이전트가 증명할 수 있도록 정형 검증 스킬을 만드시겠어요? `/create-formal-verification-skill`로 생성해 드릴 수 있습니다." 예(yes)라고 답하면 `/create-formal-verification-skill`을 호출하세요(워크스페이스, 사용자 또는 플러그인 중 pstack이 설치된 위치에서 확인됨). 아니오(no)라고 답하면 강요하지 말고 넘어가세요.

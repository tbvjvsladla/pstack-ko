---
name: why
description: >-
  '왜 X는 이렇게 작동하나요(why does X work this way)', '왜 우리가 Y를 선택했지(why we
  picked Y)', 설계 근거, 회귀(regressions), 포스트모템, 또는 데이터에 기반한
  임계값(thresholds)을 조사할 때 사용하세요. 사용 가능한 MCP들을 탐색하고 각 근거 카테고리(소스 컨트롤, 이슈
  트래커, 장문 문서, 실시간 채팅, 인프라 옵저버빌리티, 에러 트래킹, 제품 분석 웨어하우스)에 병렬로 질의한 뒤, 결정
  사항과 트레이드오프에 대해 출처가 인용된 분석을 반환합니다. 런타임 동작에 대해서는 how를 사용하세요.
disable-model-invocation: true
---

# Why

코드 이면에 있는 동기와 의도를 조사합니다. 단짝 스킬인 `how`가 코드가 무엇을 하고 어떻게 작동하는지에 답한다면, `why`는 어떤 힘들이 작용하여 현재의 형태가 만들어졌는지에 답합니다.

아래의 각 서브에이전트 생성(spawn)은 `pstack-models.mdc` 규칙의 역할 줄과 기본값을 지정합니다. `model`을 해당 줄의 값으로 설정하거나, 규칙이나 해당 줄이 없으면 기본값으로 설정하세요. 값이 `auto` 또는 `inherit-parent`일 때는 `model`을 설정하지 않은 채로 두세요. Task 도구가 슬러그를 거부하면 기본값을 사용하고 그 사실을 알리세요. 기본값도 거부하면 에러 메시지에서 동일한 패밀리의 가장 가까운 유효한 슬러그를 사용하세요. **poteto-mode** 스킬의 Subagents 섹션에 따라 작업에 맞는 각 서브에이전트의 추론 수준과 속도(빠름 또는 표준)를 선택하세요.

모든 서브에이전트 생성은 `generalPurpose`에 `readonly: false`(에이전트 모드)로 실행합니다. **절대 readonly/Ask 모드를 사용하지 마세요.** 이 모드는 MCP 접근 권한을 제거하므로 MCP 기반 조사자(investigators)들과 통합자(synthesizer)의 인용 표본 검사를 작동 불능으로 만듭니다. 단, 조사자들은 여전히 어떤 파일도 수정해서는 안 됩니다.

## 운영 자세 (Operating Posture)

**신중하고 조심스러우며 정밀한 조사자**로서 행동하세요. 확실히 아는 것과 추론하는 것을 정직하게 구분하세요. 전체 확신도 프레임워크와 표현 가이드는 `references/epistemics.md`를 읽으세요. 통합자는 반드시 이를 따라야 합니다.

## 1단계. 대상과 질문 이해하기

**대상(target)**은 대개 코드 덩어리, 패턴, 기능, 또는 이름이 붙은 설계 결정입니다. **질문(question)**은 대개 설계 근거, 트레이드오프, 동기가 된 엣지 케이스, 외부 제약 조건, 죽은 코드(dead code), 또는 광범위한 히스토리 조사입니다.

만약 대상이 모호하다면(명확한 지시 대상 없이 "왜 우리가 이걸 이렇게 하지?"라고 묻는 경우), 대화 맥락(열려 있는 파일, 최근 편집 내역, 커서 위치, 방금 논의한 내용)에서 최선의 추측을 내리세요. 추측이 빗나갔을 때 사용자가 바로잡을 수 있도록 해석한 바를 짧게 밝힌 뒤 진행하세요.

## 2단계. 코드 앵커(Code Anchor) 확립하기

조사자들을 띄우기 전에 시드 맥락을 인라인으로 먼저 구축하세요: 파일 경로와 줄 범위, 핵심 심볼(함수, 클래스, 상수), 대상을 건드린 최근 몇 개의 커밋, 머지 커밋에서 추출한 PR 번호(제목 줄의 `(#1234)` 패턴), 연결된 모든 티켓 ID입니다. 마지막으로 수정한 커밋을 보려면 `git blame -L <start>,<end> <file>`을, 파일 이름 변경을 거친 패치 포함 전체 이력을 보려면 `git log --follow -p -- <file>`을, PR 번호가 보이는 최근 커밋들을 보려면 `git log --oneline -20 -- <file>`을, PR 번호를 추출하려면 `git log -1 --format=%B <commit>`을 실행하세요. 그런 다음 실질적인 변경이 담긴 모든 커밋에 대해 PR 본문과 논의를 가져오세요: `gh pr view <number> --json title,body,author,createdAt,mergedAt,labels,closingIssuesReferences,comments,reviews`. 이 코드 앵커는 모든 조사자와 통합자에게 전달됩니다.

## 3단계. 병렬 조사자(Investigators) 실행하기

**전체 병렬 조사를 기본값으로 삼으세요.** 목표는 최소한의 맵이 아니라 완전한 커버리지 맵을 얻는 것입니다. 검색을 건너뛰지 말고 결과가 없음(null)을 문서화하세요.

### 탐색 (Discovery)

조사자들을 띄우기 전에 Cursor 환경에서 사용 가능한 MCP 목록을 확인하세요. 가용 도구 맵이 있다면 이를 사용하고, 그렇지 않다면 활성화된 MCP 서버를 위해 Cursor가 노출하는 `mcps/` 디렉터리를 점검하세요.

사용 가능한 각 MCP를 다음 일곱 가지 근거 카테고리 중 하나에 매핑하세요:

1. 소스 컨트롤 히스토리 (Source control history)
2. 이슈 / 티켓 트래커 (Issue / ticket tracker)
3. 장문 문서 (Long-form documents)
4. 실시간 팀 채팅 (Real-time team chat)
5. 인프라 옵저버빌리티 (Infrastructure observability)
6. 에러 / 예외 트래킹 (Error / exception tracking)
7. 제품 분석 웨어하우스 (Product analytics warehouse)

소스 컨트롤은 git과 `gh`를 통해 언제나 사용 가능합니다. 나머지 여섯 가지는 MCP 이름, 서버 지침, 도구 이름, 리소스 설명자를 사용해 분류하세요. 하나의 MCP가 둘 이상의 카테고리에 들어맞을 수 있다면 주된 근거에 부합하는 카테고리를 선택하세요. 모호한 사례는 커버리지 맵에 기록하세요.

일치하는 MCP가 있는 카테고리마다 한 명의 조사자를 띄우되, 동시에 실행되도록 단일 메시지에서 모두 호출하고 각 조사자가 정확히 하나의 도구 또는 MCP만 소유하게 하세요. 한 에이전트에게 여러 MCP를 담당하라고 요청하지 마세요. `model`: `why investigators` 줄(기본값 `grok-4.7-xhigh-fast`).

각 조사자의 프롬프트는 내용을 채운 `references/investigator-prompt.md`에 다음 항목들을 더한 것입니다:
- 선택된 MCP에 맞게 `references/source-playbook.md`의 예시를 응용한 카테고리 플레이북 `references/sources/<source>.md`
- **대상 코드가 방어적인 형태를 띠고 있는 경우**(null 체크, 재시도 로직, 타임아웃 처리, 속도 제한(rate limiting), 피처 플래그, 이그레스 가드, OOM 핸들러) `references/sources/incident-postmortem.md`
- 코드 앵커와 사용자의 원래 질문

### 가용한 근거 카테고리별 조사자 명단 (카테고리당 1명)

아래 각 카테고리는 "강점(Best at)" 뒤에 표시된 특정 형태의 "이유(why)"를 드러내는 데 독보적인 위치를 차지합니다. 무엇을 기대해야 할지 파악하고, 카테고리가 빈 결과를 반환할 때 공백을 명명하며, 아주 드물게 명백히 무관함이 입증되는 경우에만 건너뛰기를 정당화하는 데 이를 활용하세요.

1. **소스 컨트롤 (Source control)** (git 히스토리, PR용 `gh`, 코드 주석, 테스트). 유일하게 보장되는 소스이므로 항상 실행합니다. **강점:** *코드 리뷰 과정에서 포착된 구현 시점의 근거*.
2. **이슈 / 티켓 트래커 (Issue / ticket tracker)** (예: Linear, Jira, GitHub Issues, Plane, Shortcut MCP). **강점:** *제품 또는 비즈니스 차원의 강제 요인(forcing function)*. 이유가 엔지니어링 외부에 있을 때 가장 강력합니다.
3. **장문 문서 (Long-form documents)** (예: Notion, Confluence, Google Docs, Coda MCP). **강점:** *코드가 되기 전에 글로 작성된 장문의 설계 근거*.
4. **실시간 팀 채팅 (Real-time team chat)** (예: Slack, Discord, Microsoft Teams, Mattermost MCP). **강점:** *문서로 남겨지지 않은 실시간 숙의 과정*. 특히 소스 컨트롤, 티켓, 문서 기록이 빈약할 때 중요합니다.
5. **인프라 옵저버빌리티 (Infrastructure observability)** (예: Datadog, New Relic, Honeycomb, Grafana, Splunk MCP). **강점:** *코드 작성의 동기가 된 인프라 및 런타임 현실*. 대상 코드가 인프라 신호(타임아웃, 재시도, 속도 제한, 서킷 브레이커)에 반응할 때 가장 강력합니다.
6. **에러 / 예외 트래킹 (Error / exception tracking)** (예: Sentry, Rollbar, Bugsnag, Airbrake MCP). **강점:** *방어적이거나 교정적인 코드 작성의 동기가 된 구체적인 예외 및 에러 궤적*. catch 블록, null 가드, 타입 체크, 재시도 및 기타 방어 코드에 가장 강력합니다.
7. **제품 분석 웨어하우스 (Product analytics warehouse)** (예: Databricks, Snowflake, BigQuery, ClickHouse, dbt, Redshift MCP). **강점:** *코드의 형태를 결정지은 제품 및 데이터 현실*. 플래그로 제어되는 코드, 실험 주도 배포, 데이터 마이그레이션, 그리고 "이 숫자는 어디서 나왔는가" 같은 질문에 가장 강력합니다.

### 조사자를 건너뛸 수 있는 경우

최종 조회한 출처(Sources Consulted) 섹션에 들어갈 **명시적이고 서면화된 정당화 사유**가 있을 때만 건너뛸 수 있습니다. 유효한 사유는 두 가지뿐입니다:

- 이 환경에서 **해당 카테고리에 사용할 수 있는 MCP가 없는 경우**. 이를 선택이 아닌 공백(gap)으로 표시하세요: "실시간 팀 채팅 건너뜀. 일치하는 MCP가 없어 대화 기록을 검색할 수 없었음."
- 단순히 "아마 관련 없을 것"이 아니라 **소스가 무관함이 증명 가능한(provably irrelevant) 경우**. 기준이 매우 높습니다: "에러 / 예외 트래킹 건너뜀. 대상이 런타임 코드 경로가 없는 빌드 시점 스크립트임."

범위 평가 결과 대상이 단일 커밋의 사소한 변경이고 PR 설명에 이미 완전한 답이 들어 있다면, 가용한 일곱 가지 카테고리 검색이 모두 중복임을 **확인한 후에만** 인라인으로 답할 수 있습니다. 그 사실을 명시적으로 밝히세요. 이런 경우는 드물어야 합니다.

## 4단계. 통합하기 (Synthesize)

통합자(synthesizer)를 한 명 띄웁니다. `model`: `why synthesizer` 줄(기본값 `claude-opus-5-5-xhigh`). 통합자에게 전달할 항목:
1. 결과 없음(null)과 정당화 사유와 함께 건너뛴 카테고리를 포함한 모든 조사자의 발견 사항
2. 코드 앵커
3. 사용자의 원래 질문
4. `references/epistemics.md`의 인식론(epistemics) 프레임워크
5. `references/synthesizer-prompt.md`의 통합자 프롬프트 템플릿

## 5단계. 제시하기 (Present)

통합자의 출력을 사용자에게 제시하세요. 명확성을 위해 가볍게 다듬거나 대화 맥락을 덧붙일 수는 있지만, **확신도 표현(confidence language)은 절대 다시 쓰지 마세요**.

## 출력 형식 (Output Format)

구조는 `references/synthesizer-prompt.md`에 있는 구성을 따릅니다: 질문(The Question), 대상 코드(The Code in Question), 발견한 사실(What We Found), 합리적으로 추론할 수 있는 내용(What We Can Reasonably Infer), 경쟁 가설(Competing Hypotheses), 알지 못하는 내용(What We Don't Know), 조회한 출처(Sources Consulted), 확신도 요약(Confidence Summary). 필요에 따라 조정하되 확신도 구분은 온전히 유지하고, 조회한 출처는 아무것도 반환하지 않았거나 건너뛴 항목을 포함하여 조사자당 한 줄씩 사유와 함께 유지하세요.

조회한 출처 뒤에는, 만약 사용자의 `why` 질문이 이 코드를 변경하기 위한 사전 작업이라면 조사된 계보(lineage) 발견 사항들을 변경 계획 수립을 위한 보존(Preserve) / 변경(Change) / 회피(Avoid) / 위험(Risk) 제약 조건 세트로 변환하여 덧붙이세요.

## 피해야 할 흔한 실패 모드

- **최신성 편향 (Recency bias)**. 가장 최근 커밋이 곧 권위 있는 정답이라고 가정하는 오류입니다. 코드의 현재 형태는 대개 이전의 수많은 결정이 켜켜이 쌓인(accretion) 결과입니다. 과거로 끝까지 추적하세요.

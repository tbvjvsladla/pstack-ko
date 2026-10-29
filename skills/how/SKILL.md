---
name: how
description: >-
  "X는 어떻게 동작하는가(how does X work)", 무언가를 변경하기 전의 코드 워크스루, 그리고 배치 / 소유권
  / 계층화 질문("이 코드는 어디에 두어야 하는가", "어느 패키지가 이것을 소유하는가", "여기가 올바른 계층인가")에
  사용하세요. 서브시스템 아키텍처, 런타임 흐름, 온보딩 멘탈 모델을 설명합니다. 동기와 배경 이유가 필요할 때는 why를
  사용하세요.
disable-model-invocation: true
---

# How

"X는 어떻게 동작합니까?"라는 질문에 답하기 위해 코드베이스를 탐색합니다. 서브시스템에 새로 합류한 시니어 엔지니어의 눈높이에 맞춘 아키텍처 설명을 작성하세요 — 실무에 바로 쓸 수 있는 멘탈 모델을 구축하기에 충분하되, 주석 달린 소스 코드를 그대로 읽는 것처럼 너무 자잘해서는 안 됩니다.

모든 서브에이전트는 `subagent_type: generalPurpose` 및 `readonly: true`를 설정한 Task 서브에이전트로 띄웁니다. 탐색자(explorer)들은 `pstack-models.mdc` 규칙의 `how explorer` 줄(기본값 `grok-4.7-xhigh-fast`)을 사용하고, 모든 설명자(explainer)는 `how explainer` 줄(기본값 `claude-opus-5-5-xhigh`)을 사용합니다. `model`을 해당 줄의 값으로 설정하거나, 규칙 파일이나 해당 줄이 없다면 기본값으로 설정하세요. 값이 `auto` 또는 `inherit-parent`인 경우에는 `model`을 설정하지 않고 비워 둡니다. 만약 Task 도구가 슬러그를 거부하면 기본값을 사용하고 그 사실을 알리세요. 기본값마저 거부하면 에러 메시지에서 확인되는 동일 패밀리의 가장 가까운 유효 슬러그를 사용하세요. **poteto-mode** 스킬의 Subagents 섹션에 따라 작업에 맞게 각 서브에이전트의 추론 수준과 속도(fast 또는 standard)를 선택하세요.

## 1. 복잡도 평가 (Assess complexity)

범위가 모호하다면 당신의 해석을 먼저 밝히고 탐색을 시작하세요. 사용자가 필요하다면 방향을 수정해 줄 것입니다.

- **단순함 (Simple)** (단일 모듈, 작은 유틸리티, "함수 X는 어떻게 동작하는가" 같은 좁은 질문): 탐색자(explorer)를 따로 띄우지 않습니다. `references/explainer-prompt.md`에서 탐색자 발견 사항(explorer-findings) 섹션을 제외한 프롬프트를 사용하여, 한 번의 패스로 직접 탐색하고 설명까지 작성하는 설명자(explainer) 하나만 띄웁니다. 4단계로 이동합니다.
- **복잡함 (Complex)** (여러 파일이나 서비스에 걸쳐 있는 서브시스템, 횡단 관심사 기능, 전체 아키텍처 개요): 2단계로 이동합니다.

단순한지 복잡한지 의심스러울 때는 단순한 경로를 택하세요.

## 2. 탐색 (Explore - 복잡한 경우에만)

질문을 각각 서브시스템의 서로 다른 단면(slice)을 다루는 2~4개의 관점(angles)으로 분해합니다. `references/explorer-prompt.md`에 각자의 관점을 채워 넣어 단일 메시지에서 모든 탐색자를 동시에 띄웁니다.

## 3. 종합 (Synthesize - 복잡한 경우에만)

모든 탐색자가 결과를 반환하면 `references/explainer-prompt.md`에 모든 탐색자의 발견 사항을 채워 넣어 설명자(explainer) 하나를 띄웁니다.

## 4. 제시 (Present)

설명자의 산출물을 제시합니다. 대화 맥락을 반영하거나 명확성을 높이기 위한 가벼운 편집은 괜찮지만, 내용을 대폭 다시 쓰지는 마세요.

## 출력 형식 (Output format)

설명은 `references/explainer-prompt.md`에 정의된 섹션 구조를 사용하며, 해당하지 않는 섹션은 생략합니다: Overview, Key Concepts, How It Works, Where Things Live, Gotchas.

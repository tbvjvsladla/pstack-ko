# Bugbot 분류 (Bugbot triage)

Babysit 플레이북(`../playbooks/babysit.md`)에서 Bugbot이나 리뷰 자동화 코멘트를 처리할 때 이 레퍼런스를 사용하세요. 목표는 기본적으로 Bugbot을 무시하는 것이 아닙니다. 모든 코멘트를 필수 코드 변경 요청으로 취급하는 일을 멈추는 것이 목표입니다.

## 의사결정 루브릭 (Decision rubric)

행동하기 전에 각 Bugbot 스레드를 다음 중 하나로 분류하세요:

- `fix`: 코멘트가 타당한 정확성, 보안, 개인정보 보호, 데이터 손실, 인증(auth), 과금(billing), 마이그레이션, 멱등성, 경쟁 상태(race), 또는 배포 동작 문제를 지적합니다. 해당 코드를 소유한 가장 아래쪽 PR에서 수정한 다음, 커밋 SHA와 함께 답변을 달고 스레드를 해결(resolve) 처리하세요.
- `dismiss`: 코멘트가 문서화된 저위험 노이즈 패턴과 일치하며, 현재 코드 및 컨텍스트가 코드 변경이 필요 없음을 증명합니다. 짧은 이유를 달아 답변하고 스레드를 해결 처리하세요.
- `ask`: 코멘트가 새로운 유형이거나, 심각도가 높거나, 보안/개인정보/데이터와 관련되어 있거나, 모호합니다. 추측하지 말고 사용자에게 질문하세요.

의심스러울 때는 질문(`ask`)하세요. 시끄러운 코드 품질 코멘트를 건너뛰는 비용은 저렴하지만, 실제 데이터나 보안 버그를 건너뛰는 대가는 저렴하지 않습니다.

## 학습된 패턴 형식 (Learned pattern format)

향후 패턴을 추가할 때는 다음 형식으로 작성하세요:

```markdown
### <짧은 패턴 이름>

- Confidence: candidate | recurring | strong
- Skip when: <참이어야 하는 조건들>
- Do not skip when: <리스크 경계 조건들>
- Example signal: <패턴을 식별하는 문구나 코드 컨텍스트>
- Source: <PR/코멘트 URL 또는 짧은 히스토리 노트>
```

사례가 한두 개일 때는 `candidate`를 사용하세요. 실제 기각 사례가 여러 번 누적된 후에는 `recurring`을 사용하세요. 패턴이 좁고 명확하며 반복적으로 검증되었고 리스크가 낮을 때만 `strong`을 사용하세요.

## 반복되는 건너뛰기 후보 (Recurring skip candidates)

### 의도적인 UI 또는 디자인 시스템 시각적 변경 (Intentional UI or design-system visual changes)

- Confidence: candidate
- Skip when: PR 설명, 스크린샷, 디자인 리뷰, 또는 주변 코드에서 시각적 변경이 명시적으로 드러나며, Bugbot 코멘트가 단지 공유 시각적 기본값이 바뀌었다는 사실을 재진술하고 있을 뿐일 때.
- Do not skip when: 코멘트가 접근성, 포커스 가시성, 키보드 내비게이션, 색상 대비, 또는 PR이 의도적으로 변경하지 않은 컴포넌트 API 계약을 지적할 때.
- Example signal: 포커스 아웃라인, 버튼 크기, 간격, 또는 공유 컴포넌트 시각적 기본값에 대한 코멘트에 소유자가 "의도된 변경(intentional / intended)"이라고 답변하는 경우.

### Bugbot이 볼 수 없는 상위 스택 또는 스택 로컬 사용처 (Upstack or stack-local usage Bugbot cannot see)

- Confidence: candidate
- Skip when: Bugbot이 export, 컴포넌트, 헬퍼, 또는 파일을 미사용(unused)으로 표시했으나, 활성 포지의 PR 목록 및 diff, 상위 스택 diff, 또는 PR 컨텍스트를 통해 스택 내 후속 PR에서 사용됨이 확인될 때.
- Do not skip when: 현재 PR이 스택의 일부가 아니거나, 심볼이 공개(public) API이거나, 상위 스택에서 사용된다는 주장을 검증할 수 없을 때.
- Example signal: "Exported component is never used" 코멘트에 사람이 "used upstack(상위 스택에서 사용됨)"이라고 답변하는 경우.

### 병렬 구현 중의 일시적인 중복 (Temporary duplication during parallel implementation)

- Confidence: candidate
- Skip when: 삭제, 교체, 또는 검증 중인 기존 경로와 나란히 새 경로를 유지하기 위해 PR이 의도적으로 소량의 코드를 중복 작성했을 때.
- Do not skip when: 중복된 코드가 보안, 과금, 데이터 접근, API 동작을 변경하거나, 장기 유지되는 공유 추상화가 명백히 리스크를 줄여줄 때.
- Example signal: "Significant duplication" 또는 "duplicated validation logic" 코멘트에 소유자가 기존 경로는 곧 삭제될 예정이거나 중복 로직이 의도적으로 국소화된 것이라고 설명하는 경우.

### 기존 프레임워크 또는 컴포넌트 불변식이 경고를 이미 커버함 (Existing framework or component invariant covers the warning)

- Confidence: candidate
- Skip when: 우려 사항이 현재 diff나 주변 코드에서 확인할 수 있는 공유 컴포넌트, 프레임워크 계약, 타입 불변식, 또는 단일 진실 공급원(single source of truth)에 의해 이미 보장될 때.
- Do not skip when: 불변식이 가정되었을 뿐 강제되지 않거나, 타이밍에 의존하거나, 값이 어긋날 수 있는 비동기/상태 경계를 가로지를 때.
- Example signal: 공유 팝오버가 뷰포트 경계를 강제하는데 내부 팝오버에 `max-height`가 없다고 지적하는 코멘트, 또는 로컬에서 체크한 값과 전달된 값이 동일한 소스를 공유하는 nullable 값에 대한 코멘트.

### 소유자가 선언한 후속 작업 또는 지연된 정리 (Owner-declared follow-up or deferred cleanup)

- Confidence: candidate
- Skip when: PR 소유자가 해당 이슈를 알려진 후속 작업(follow-up)이라고 명시적으로 밝히고, 현재 PR로 인해 동작이 더 나빠지지 않으며, 코멘트가 고위험 영역에 관한 것이 아닐 때.
- Do not skip when: 에이전트가 소유자의 의견 없이 단독으로 행동 중이거나, 이슈가 중간/높은 심각도의 제품 동작이거나, 미룰 경우 새로운 회귀(regression)가 머지될 때.
- Example signal: "I'll worry about that later" 또는 "we'll delete this eventually".

### 자체 철회되었거나 명시적인 오탐(false-positive) 규칙 코멘트 (Self-withdrawn or explicit false-positive rule comments)

- Confidence: recurring
- Skip when: 코멘트 본문이나 이후 Bugbot의 답글이 해당 지적 사항이 철회되었거나 규칙을 준수하고 있거나 오탐이라고 명시적으로 밝히고, 에이전트가 관련 규칙을 로컬에서 검증할 수 있을 때.
- Do not skip when: 고위험 이슈에 대해 아무 설명 없이 사람이 "false positive"라고 말한 것만이 유일한 증거일 때.
- Example signal: 본문 자체에 해당 파일이 이미 규칙을 준수하고 있다고 적혀 있는 파일 명명 규칙 코멘트.

## 기본적으로 질문할 항목 (Ask by default)

이전 PR에서 유사한 코멘트를 기각한 적이 있더라도 다음 카테고리는 자동으로 건너뛰지 마세요:

- 보안, 개인정보 보호, 인증, 과금, 데이터 보존, 학습 데이터, 권한 경계 관련 지적 사항.
- 높은 심각도(High-severity)의 지적 사항.
- 마이그레이션, 스키마, 멱등성, 동시성, 시스템 간 동작 관련 지적 사항.
- 제안된 수정이 작고 제품 의도를 바꾸지 않으면서 명백히 리스크를 줄여주는 코멘트.

과거 데이터를 보면 사람이 보안이나 데이터 흐름 코멘트를 기각하는 경우도 있었습니다. 그러한 사례는 팀 전체의 건너뛰기 규칙이 아니라 소유자의 개별 판단으로 취급하세요.

## 최근 베이비시팅에서 얻은 후보 학습 사항 (Candidate learnings from recent babysits)

베이비시팅 도중이나 이후에 팀에 유용해 보이지만 아직 완전히 성숙하지 않은 새로운 후보 학습 사항이 있다면 여기에 추가하세요. 여러 PR을 통해 패턴이 확인되면 위 섹션의 반복 후보(recurring candidates)로 승격시키는 것을 권장합니다.

### 네이티브 브라우저 동작의 수동 재구현 (Manual reimplementations of native browser behavior)

- Confidence: candidate
- Skip when: 사실상 건너뛰지 마세요. diff가 네이티브 브라우저 동작을 수동 구현체로 대체할 때(네이티브 sticky → JS로 위치를 잡는 클론, 네이티브 스크롤 타겟팅 → 전달된 wheel/touch 이벤트, 페인트 순서 가림(occlusion) → 마스크/clip-path), 해당 코드에 대한 Bugbot의 로직 버그 지적은 일관되게 타당했습니다.
- Do not skip when: 지적 사항이 이벤트 전달 누락(wheel `deltaMode`, 터치 패닝, 가장자리에서의 스크롤 체이닝, 탭 슬롭(tap slop)), 마스크/클립 히트 테스팅 불일치, 또는 그러한 코드 내의 Observer 대 React 상태 타이밍 경쟁 상태를 다룰 때. 기본적으로 수정(`fix`)하세요.
- Example signal: "masks do not affect hit-testing", "overlay blocks wheel scroll", "ignores deltaMode", "runs in the IntersectionObserver callback before React applies state".
- Source: 하나의 sticky-occlusion PR: 6번의 Bugbot 패스, 약 18개의 지적 사항이 나왔으며 기각 없이 전부 수정됨.

### 계약 테스트 드리프트 주장은 저렴하게 검증 가능함 — 테스트를 먼저 실행할 것 (Contract-test drift claims are cheaply verifiable — run the test first)

- Confidence: candidate
- Skip when: 검증 자체는 명령어 한 줄이면 되므로 절대 건너뛰지 마세요. PR이 프로토콜이나 문서 산문을 고정하는 계약 테스트(SKILL.md에 대한 정규식, 문서 문구 스냅샷)를 배포할 때 Bugbot이 "테스트가 더 이상 문서와 일치하지 않는다"(또는 그 반대)고 주장한다면, 분류하기 전에 PR 최신 팁(tip)에서 해당 테스트를 먼저 실행해 보세요. 실행 결과가 레드(실패)이면 주장이 경험적으로 확인된 것이고, 그린(통과)이면 기각 답변에 사용할 구체적인 반증이 됩니다.
- Do not skip when: 해당 없음(n/a) — 이것은 기각 패턴이 아니라 검증 지름길입니다. 반복 패스에서 기각 쪽으로 무게를 두는 휴리스틱이 여기서는 오작동할 수 있음에 유의하세요: 산문을 고정하는 테스트는 바로 이전 수정 라운드들이 산문을 편집했기 때문에 드리프트가 발생합니다.
- Example signal: 이전 수정 커밋들이 고정된 문구를 다시 다듬은 PR에서 발생한 "Contract test omits the pre-fix wait" 코멘트; 최신 팁에서 테스트를 실행하자 정확히 지적된 단언문(assertion)에서 실패함.
- Source: 8번의 Bugbot 패스를 거친 하나의 산문 고정 PR; 이전의 모든 패스가 수정 및 해결되었음에도 7번째 패스의 주장이 실제 결함이었음.

### 동일한 PR의 후속 커밋에서 이미 수정된 오래된 보안 리뷰 지적 사항 (Stale security-review finding already fixed later in the same PR)

- Confidence: candidate
- Skip when: 에이전틱 보안 리뷰(또는 유사 도구)가 인가(authz)/유효성 검사 호출 누락을 주장하지만, 현재 PR의 최신 팁에는(보통 리뷰가 실행된 이후 추가된 후속 강화(hardening) 커밋을 통해) 테스트와 함께 정확히 그 게이트가 명확히 포함되어 있을 때.
- Do not skip when: 인용된 헬퍼가 논의 중인 주체(principal)에 대해 아무 동작도 하지 않거나(no-op), 체크가 보호 대상 부수 효과(side effect) 이후에 실행되거나, 지적된 주체에 대한 테스트 커버리지가 누락되었을 때.
- Example signal: 최신 팁에서는 이미 부수 효과 이전에 정확한 가드가 호출되고 있는데도 남아 있는 HIGH 등급의 "missing authorization check" 지적 사항.
- Source: 강화 커밋이 리뷰 실행 시점보다 나중에 푸시된 하나의 웹훅 엔드포인트 PR.

### 의도적으로 좁게 설정한 에러 조건을 넓히면 실제 에러가 가려짐 (Widening a deliberately narrow error condition would mask the real error)

- Confidence: candidate
- Skip when: 지적 사항이 좁은 에러 조건(특정 `errno`, 에러 코드, 또는 상태 클래스)을 포괄적 조건(catch-all)으로 넓히라고 요구하지만, 그 좁은 조건이 실제 의미 있는 구분을 인코딩하고 있을 때. 대표적인 형태는 `ENOENT`로 게이트된 의존성 폴백입니다: "바이너리가 설치되지 않음"은 "명령어가 실행되었으나 실패함"과는 전혀 다른 상황입니다. 0이 아닌 모든 종료 코드에서 재시도하게 만들면 정당한 실패(대상 없음, 만료된 인증, 네트워크 오류)까지 폴백에 대해 다시 실행한 뒤 폴백의 에러를 보고하게 되어 진짜 원인을 숨기게 됩니다.
- Do not skip when: 좁은 조건이 **동일한** 카테고리의 케이스(`EACCES` 같은 또 다른 "바이너리 사용 불가" `errno`나 또 다른 전송 계층 실패)를 놓치고 있거나, 처리되지 않은 경로가 데이터를 잃거나 부분 상태를 남기거나, 재시도가 멱등성을 가지며 원래 에러도 함께 노출될 때.
- Example signal: 실패한 작업이 아니라 누락된 의존성을 위해 존재하는 폴백 코드를 가리키며 "only retries when X fails with ENOENT … never tries the fallback even when a working Y exists"라고 지적하는 코멘트.
- Source: 실패한 명령어가 아니라 누락된 바이너리를 위해 폴백이 존재했던 하나의 CLI 이름 변경 PR.

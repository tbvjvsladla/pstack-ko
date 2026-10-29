# 변경 사항 구현 및 diff 정리하기

빌드 플레이북들은 하나의 규율을 공유합니다. 관찰한 바를 말하고, 플레이북이 증거를 요구하게 두는 것입니다. 이 페이지에서는 자주 쓰이는 각 빌드 과업의 프롬프트에 무엇을 담아야 하는지 살펴보고, 이어서 diff를 리뷰하기 좋은 상태로 유지해 주는 정리(cleanup) 습관을 다룹니다.

## 알고 있는 사실로 각 빌드 플레이북에 프롬프트 주기

버그 프롬프트는 증상을 기술하고 먼저 재현을 요청합니다:

```text
/poteto-mode this command emits two records after a retry. repro first, then fix and verify.
```

기능(Feature) 프롬프트는 새로운 동작과 바뀌지 말아야 할 것을 기술합니다:

```text
/poteto-mode add a --json flag. text output stays byte-identical. verify both forms.
```

리팩터링 프롬프트는 구조를 옮기기 전에 동작을 고정(pin)합니다:

```text
/poteto-mode move parsing into one module, zero behavior change. record the current output first and prove it's unchanged after.
```

성능(Perf) 프롬프트는 막연한 인상이 아니라 측정치를 기술합니다:

```text
/poteto-mode startup takes 1.8s on this fixture. trace it, fix the measured cause, show me before and after.
```

이 프롬프트들은 각각 해당 플레이북([Bug fix](../../skills/poteto-mode/playbooks/bug-fix.md), [Feature](../../skills/poteto-mode/playbooks/feature.md), [Refactoring](../../skills/poteto-mode/playbooks/refactoring.md), [Perf issue](../../skills/poteto-mode/playbooks/perf-issue.md))으로 라우팅되며, 플레이북이 여러분이 일일이 타이핑하지 않은 단계들(수정 전 재현, 구현 전 데이터 형태 명명, 구조 변경 전 동작 고정, 최적화 전 프로파일링)을 채워 넣습니다.

단일 수치를 지속적으로 개선해야 할 때는 [Hillclimb 플레이북](../../skills/poteto-mode/playbooks/hillclimb.md)이 있습니다. 지표, 목표치, 최소 시도 횟수를 주면 고정된 측정 하네스를 가지고 한 번에 하나의 가설씩 루프를 돕니다. 개선된 결과(win)는 남기고 나머지는 모두 되돌립니다.

두 성능 플레이북은 모두 측정 수치에 대해 [`/benchmark-checklist`](../../skills/benchmark-checklist/SKILL.md)를 실행합니다. Perf issue는 베이스라인과 그 이후의 모든 수치를 검증하고, Hillclimb는 측정 하네스를 고정하기 전에 이를 검증합니다. 직접 이 스킬을 입력해야 할 시점은 [검증 및 배포하기](./06-verify-and-ship.md#vet-a-measured-number-with-benchmark-checklist)에서 보여줍니다.

때로는 수정에 앞서 원인부터 먼저 파악하고 싶을 때가 있습니다. 메모리 누수, 유휴 상태 CPU 스핀, 시각적 글리치 같은 라이브 증상의 경우 [Runtime forensics 플레이북](../../skills/poteto-mode/playbooks/runtime-forensics.md)이 실행 중인 프로세스를 계측합니다. 이미 캡처해 둔 프로파일이 있는 경우 [Trace forensics 플레이북](../../skills/poteto-mode/playbooks/trace-forensics.md)이 아티팩트를 읽고 핫 프레임(hot frame)을 소스 코드에 매핑합니다. 두 플레이북 모두 수정이 아니라 진단 결과를 반환합니다:

```text
/poteto-mode here's a cpuprofile from the slow startup. tell me where the time goes and which source lines own it. no fix yet.
```

## `/tdd`로 실패하는 테스트부터 먼저 작성하기

버그에 저렴한 로컬 테스트 경로가 있을 때는 전체 프롬프트가 단 두 단어면 충분합니다:

```text
/tdd implement
```

맥락이 이미 주어져 있다면 그것으로 충분합니다. [`/tdd`](../../skills/tdd/SKILL.md)는 의도한 이유로 실패하는 가장 작은 테스트를 먼저 작성한 뒤, 수정을 적용하고, 테스트를 다시 실행합니다. 테스트를 작성하려면 광범위한 하네스 셋업이나 깨지기 쉬운 모킹(mock)이 필요한 경우, 스킬은 그 사실을 알리고 가장 가까운 실행 가능한 검사 수단을 대신 사용합니다. 실제 명령어 실행이 더 강력한 증거가 되는 곳에 억지로 단위 테스트를 강제하지 마세요.

## 이름으로 TypeScript 규칙 불러오기

[`typescript-best-practices`](../../skills/typescript-best-practices/SKILL.md)는 타입 시스템 원칙들을 구별된 유니온(discriminated unions), 경계에서의 `unknown` 사용, 배리언트 완전성 검사(exhaustive variants), 스키마 파생 타입 같은 구체적인 규칙으로 풀어냅니다. 이 스킬은 자동으로 로드되지 않으므로 과업이 `.ts` 또는 `.tsx` 파일을 건드릴 때 `/typescript-best-practices`를 직접 입력하세요.

## 커밋하기 전에 정리하기

[Opening a PR 플레이북](../../skills/poteto-mode/playbooks/opening-a-pr.md)은 각 커밋 전에 diff에 대해 `/deslop`을 실행하고 PR 설명과 커밋 본문에 [`/unslop`](../../skills/unslop/SKILL.md)을 적용합니다. `/deslop`은 pstack이 아니라 `cursor-team-kit` 플러그인에 포함되어 있습니다. 해당 플러그인이 없다면 평이한 말로 동일한 결과를 요청하세요: 코드를 중계하는 주석, 근거 없는 가드 로직, 죽은 호환성 경로, 관련 없는 편집을 제거하라고 지시하면 됩니다.

산문의 경우 `/unslop`은 대상과 여러분이 원하는 추가 규칙을 인자로 받습니다:

```text
/unslop the readme changes, no emdashes
```

쓰다 보면 자신만의 축약 표현이 생길 것입니다. 이 스킬은 `unslop that, tighten it` 같은 짧은 프롬프트에서도 의도를 잘 읽어냅니다.

## `/no-comments`로 주석 걷어내기

주석에는 별도의 패스가 필요하며, 그 주석을 작성한 에이전트가 직접 맡아서는 안 됩니다. 작성자는 여러분이 자신의 주석을 변호하듯 자기 주석을 변호하기 마련입니다. 따라서 리뷰 전에 새로운 눈에 주석을 넘기세요:

```text
/no-comments the diff
```

[`/no-comments`](../../skills/no-comments/SKILL.md)는 짧은 보존 목록(라이선스 헤더, 퍼블릭 API의 문서 주석, 코드가 설명할 수 없는 바를 설명하는 링크, 우리가 구조를 바꿀 수 없는 외부 의존성이 강제하는 동작)만을 가진 읽기 전용 리뷰어인 [Comment Sicko](../../agents/comment-sicko.md)를 스폰합니다. 그 외의 모든 주석은 제거됩니다. 우리 코드 내부의 의외성에는 그런 예외가 허용되지 않습니다. 해당 주석은 리팩터링 플래그로 되돌아오며, `/no-comments`는 수용한 플래그들을 근본 원인 지점에서 수정합니다. 주석이 "do not remove" 같은 제약 조건을 주장하는 경우, 스킬은 그 주장을 타입, 테스트 또는 린트로 인코딩할 방법을 제안합니다. 어느 쪽이든 주석 자체는 사라집니다.

역할 분담을 명확히 알아둘 가치가 있습니다. `/deslop`은 코드에서 슬롭을 걷어내고, `/unslop`은 산문에서 슬롭을 걷어내며, `/no-comments`는 주석을 작성하지 않은 별도의 리뷰어에게 주석을 넘깁니다.

**함정(Pitfall):** 코드 정리는 선택적인 막바지 광내기가 아닙니다. 코드를 중계하는 주석과 방어적인 군더더기가 섞인 diff는 리뷰어에게 미완성 코드로 읽히며, 그 불필요한 코드야말로 다음 버그가 숨어드는 곳입니다. diff가 부풀려진 느낌이 든다면 리뷰에서 지적받은 후가 아니라 커밋하기 전에 `deslop it`이라고 말하세요.

다음: [검증 및 배포하기](./06-verify-and-ship.md).

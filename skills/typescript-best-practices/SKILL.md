---
name: typescript-best-practices
description: >-
  TypeScript 모범 사례입니다. .ts 또는 .tsx 파일을 읽거나 편집할 때 사용하세요.
paths: ["**/*.ts", "**/*.tsx"]
disable-model-invocation: true
---

# TypeScript 모범 사례 (TypeScript best practices)

먼저 **type-system-discipline** 원칙 스킬을 적용하세요.

| 규칙 | 요약 |
|---|---|
| 구별된 유니온 (Discriminated unions) | 불가능한 상태가 표현될 수 없도록 `kind` 리터럴 판별자(discriminant)로 변형(variant)들을 모델링하세요. 선택적 필드(optional-field)들의 묶음을 쓰지 마세요. |
| 브랜디드 타입 (Branded types) | 원시 타입이 서로 섞이지 않도록 `& { readonly __brand: "X" }`로 브랜딩하세요. 경계에서 한 번만 검증합니다. 브랜드는 `as`가 아니라 검증을 수행하는 타입 가드(`input is AgentId`)에서 나와야 합니다. |
| 구성적 모델링 (Constructive modeling) | 잘못된 값이 아예 생성될 수 없도록 형태를 조립하세요. 비어 있지 않은 배열은 `[T, ...T[]]`, 짝수 길이는 `[T, T][]`, 범위는 `start`와 `duration`의 조합을 사용합니다. 런타임 가드나 정제 타입(refinement types)에 대한 희망 사항에 의존하지 마세요. |
| 가장 단순한 전함수 타입 (Simplest total type) | 그 위에서 수행되는 모든 연산이 전함수(total)로 유지되는 동안에는 `T[]`를 그대로 유지하세요. 느슨한 타입 때문에 `!`, 캐스팅, 또는 "절대 일어날 리 없음(should never happen)" 예외 던지기가 강제되는 곳에서만 `NonEmpty<T>`로 강화하세요. |
| `any` 대신 `unknown` (`unknown` over `any`) | 외부 데이터는 `unknown`입니다. |
| 가드보다 스키마 우선 (Schemas before guards) | 프로퍼티를 하나씩 검사하는 타입 가드를 수작업으로 작성하기 전에, 저장소의 런타임 스키마 라이브러리를 사용하고 `z.infer` 등을 통해 스키마에서 타입을 추론하세요. |
| `as` 캐스팅 금지 (No `as` casts) | 모든 `as`는 대기 중인 런타임 크래시입니다. 오직 검증 후에만 캐스팅하세요. |
| 타입 좁히기 계층 (Narrowing hierarchy) | 판별자 switch > `in` 연산자 > `typeof`/`instanceof` > 사용자 정의 타입 가드 > `as`. |
| 타입 가드 (Type guards) | 주장을 실제로 검증해야 합니다. 거짓말을 하는 가드는 안전하다는 이름 뒤에 버그를 숨기므로 `as`보다 더 나쁩니다. 이름은 `isX` 또는 `hasX`로 짓습니다. |
| 완전성 검사 (Exhaustiveness) | 새로운 변형이 추가될 때 컴파일러가 에러를 내도록 default 분기 안에 `const _exhaustive: never = x;`를 인라인으로 작성하세요. |
| `as` 대신 `satisfies` (`satisfies` over `as`) | 리터럴 타입을 넓히지(widening) 않으면서 값을 검증합니다. |
| 경계 검증 (Boundary validation) | 데이터가 들어오는 경계에서 이름이 부여된 도메인 타입으로 파싱하세요. `Record<string, unknown>`(어떻게 표기하든)은 그 파싱 지점에서 멈춥니다. 내부에서는 타입을 신뢰하세요. **boundary-discipline** 원칙 스킬을 참조하세요. |
| 스키마 파생 타입 (Schema-derived types) | 새 인터페이스를 선언하기 전에 `Pick`/`Omit`/`Parameters`/`ReturnType`/`Awaited`/`typeof`를 먼저 활용하세요. |
| 객체 인자 (Object args) | 인자 순서가 스스로 설명되도록 위치 기반 인자 대신 객체를 전달하세요. 핫 패스(프레임당 렌더링, 토크나이저, 파서)에서는 건너뛰세요. |
| 실제 테스트 (Real tests) | 직접 실행할 수 있는 것은 모킹(mock)하지 마세요. 누수/해제(leak/disposable) 검사가 포함된 프레임워크의 실제 테스트 프리미티브를 선호하고, 실행 중인 빌드에서 UI를 검증하세요. 로컬에서 실행할 수 없는 것만 모킹하세요. |
| 구조화된 텔레메트리 (Structured telemetry) | ID 하나만으로 디버깅할 수 있을 만큼 충분한 맥락을 담은 구조화된 로거 진단 정보를 선호하세요. 배포되는 코드에는 `console.log`를 넣지 마세요. |

예시: `references/patterns.md`.

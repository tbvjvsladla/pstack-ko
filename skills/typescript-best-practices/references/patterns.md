# TypeScript 패턴 (TypeScript patterns)

`SKILL.md`의 각 규칙에 대한 코드 예시입니다. 기저에 깔린 원칙들은 특정 언어에 종속되지 않습니다. **type-system-discipline** 및 **boundary-discipline** 원칙 스킬을 참조하세요.

## 브랜디드 타입 (Branded types)

원시 값들이 서로 뒤섞이지 않도록 브랜드를 부여하세요. 경계에서 한 번만 검증합니다. 다운스트림 코드는 해당 타입을 신뢰합니다.

```ts
type AgentId = string & { readonly __brand: "AgentId" };

function isAgentId(input: string): input is AgentId {
  return isUUID(input);
}

function parseAgentId(input: string): AgentId {
  if (!isAgentId(input)) throw new Error(`Invalid agent id: ${input}`);
  return input;
}

function focusAgent(id: AgentId): void {
  /* input is trusted */
}
```

`readonly __brand: 'X'` 형태를 그대로 맞추세요. 새로운 관례를 지어내지 마세요.

## 구별된 유니온 (Discriminated unions)

리터럴 판별자(discriminant)로 변형들을 모델링하세요. 모든 변형이 동일한 필드 이름을 공유하고 각 변형의 값이 고유하므로 불가능한 조합은 아예 표현될 수 없습니다.

```ts
// Don't. Boolean + optionals lets contradictory states exist.
type DiffState = { loading: boolean; diff?: GitDiff; error?: string };

// Do. Only valid states exist.
type DiffState =
  | { kind: "loading" }
  | { kind: "ready"; diff: GitDiff }
  | { kind: "error"; error: string };
```

하나의 판별자 이름(`kind`, `type`, `tag`)을 선택해 일관되게 유지하세요.

## 구성적 모델링 (Constructive modeling)

느슨한 타입을 런타임 검사로 제한하는 대신, 모두 적법한 부품들로 타입을 조립하세요.

가변 튜플(variadic tuple)을 이용한 비어 있지 않은 배열:

```ts
type NonEmpty<T> = [T, ...T[]];

// Don't: T[] plus a length check every caller must repeat
function pickWinner(entries: string[]): string {
  if (entries.length === 0) throw new Error("no entries");
  return entries[Math.floor(Math.random() * entries.length)];
}

// Do: an empty value of the type can't exist
function pickWinner(entries: NonEmpty<string>): string {
  return entries[Math.floor(Math.random() * entries.length)];
}
```

일반 `T[]`가 들어오는 지점에서는 가드로 한 번만 좁히세요. 그러면 그 사실이 타입에 실려 이동합니다:

```ts
const isNonEmpty = <T>(arr: T[]): arr is NonEmpty<T> => arr.length > 0;
```

쌍(pair)으로 표현한 짝수 길이:

```ts
type Pairs<T> = [T, T][];
```

시작 시각과 지속 시간(duration)으로 표현한 시간 범위:

```ts
// Don't: a comment holds the invariant
type TimeRange = { start: Date; end: Date }; // start <= end

// Do: a negative range can't be written; derive end when needed
type TimeRange = { start: Date; durationMs: number };
```

`durationMs`는 일반 `number`로 유지하세요. 습관적으로 브랜딩하지 말고, 지속 시간이 기대되는 자리에 날것의 숫자가 잘못 전달될 위험이 있을 때만(Branded types 규칙에 따라) 브랜딩하세요. 잘못된 상태를 생성 불가능하게 만드는 표현을 선택한 뒤, 필요한 읽기 인터페이스(`pairs.flat()`, `rangeEnd()` 헬퍼 등)를 그 위에 노출하세요.

## 가장 단순한 전함수 타입 (Simplest total type)

모든 것을 무조건 강화하지 마세요. 그 위에서의 모든 연산이 전함수(total)일 때는 `T[]`를 그대로 유지하세요:

```ts
const sum = (xs: number[]) => xs.reduce((a, b) => a + b, 0); // [] is 0, fine
```

느슨한 타입이 사용처에서 거짓말을 강제할 때만 타입을 강화하세요. 그 징후는 `!`, `arr[0] as T`, 그리고 "절대 일어날 리 없음(should never happen)" 예외 던지기입니다:

```ts
// Don't: partiality smuggled past the compiler
function newestSession(sessions: Session[]): Session {
  return sessions.at(0)!;
}

// Do: strengthen the input; the assertion disappears
function newestSession(sessions: NonEmpty<Session>): Session {
  return sessions[0];
}
```

반환 타입을 `Session | undefined`로 약화시키는 것도 전함수 시그니처를 만드는 또 다른 방법입니다.

## `any` 대신 `unknown` (`unknown` over `any`)

외부 데이터는 언제나 `unknown`입니다. 사용하기 전에 타입을 좁히세요.

```ts
// Don't
function handle(input: any) {
  return input.foo.bar;
}

// Do
function handle(input: unknown) {
  if (typeof input === "object" && input !== null && "foo" in input) {
    // narrowed; compiler verifies access
  }
}
```

외부 출처에는 RPC 페이로드, `JSON.parse`, `postMessage`, IPC, 파일 내용, 환경 변수, 데이터베이스 결과가 포함됩니다.

## 수작업 가드보다 스키마 우선 (Schemas before hand-rolled guards)

외부 데이터에 대해 프로퍼티를 하나씩 검사하는 타입 가드를 직접 작성하기 전에, 저장소의 런타임 스키마 라이브러리와 기존 스키마들을 먼저 찾아보세요. 단일 스키마가 검증을 소유하게 하고 그로부터 TypeScript 타입을 파생시키세요. 스키마, 중복 인터페이스, 타입 가드를 따로 두어 서로 어긋나게(drift) 만들지 마세요.

```ts
import { z } from "zod";

const UserSchema = z.object({
  id: z.string().uuid(),
  role: z.enum(["admin", "member"]),
});

type User = z.infer<typeof UserSchema>;

function parseUser(input: unknown): User {
  return UserSchema.parse(input);
}
```

실패가 예상된 분기 중 하나일 때는 `safeParse`를 사용하세요. 저장소가 다른 스키마 라이브러리를 사용한다면 그에 상응하는 타입 추론 헬퍼를 사용하세요. 가드 하나를 만들기 위해 새로운 스키마 의존성을 추가하지 마세요. 이 규칙은 코드베이스가 이미 신뢰하고 있는 스키마 시스템을 우선합니다.

## `as` 캐스팅 금지 (No `as` casts)

모든 `as`는 잠재적인 런타임 크래시입니다. 타입 시스템이 주장을 검증한 후에만 캐스팅하세요.

```ts
import { z } from "zod";

// Don't
const user = data as User;

// Don't
function isUser(data: unknown): data is User {
  return typeof data === "object" && data !== null && "id" in data;
}

// Do
const userSchema = z.object({ id: z.string(), name: z.string() });
type User = z.infer<typeof userSchema>;

function parseUser(data: unknown): User {
  return userSchema.parse(data);
}
```

타입이 먼저 정의되어 있을 때는 검증기(validator)에 그것이 증명하는 타입을 명시하세요. 그러면 해당 타입보다 적은 내용을 증명하는 검증기를 컴파일러가 거부합니다. 아래 객체에서 `name`을 제거하면 할당문에서 컴파일이 실패합니다.

```ts
type User = { id: string; name: string };

const userSchema: z.ZodType<User> = z.object({ id: z.string(), name: z.string() });
```

기존 코드에서 `as`를 리팩터링해 제거할 때는 왜 TypeScript가 추론하지 못하는지 원인을 파악하세요:

- 판별자 누락: 판별자를 추가하고 구별된 유니온으로 전환하세요.
- 지나치게 넓은 소스 타입(예: `Record<string, unknown>`): 타입을 좁히세요.
- 타입이 없는 경계: 형태를 소유한 스키마로 파싱하세요. 스키마가 아예 없는 곳에만 새로 추가하세요.
- 진정으로 타입 시스템으로 표현 불가능한 경우: 브랜디드 타입이나 `satisfies`를 사용하세요.

## 타입 좁히기 계층 (Narrowing hierarchy)

가장 좋은 방법부터 최후의 수단 순입니다:

1. **구별된 유니온 switch / if.** 컴파일러가 자동으로 타입을 좁힙니다.
2. **`in` 연산자.** `"key" in obj`는 해당 키를 가진 변형으로 좁힙니다.
3. **`typeof` / `instanceof`.** 원시 값과 클래스 인스턴스에 사용합니다.
4. **사용자 정의 타입 가드.** 위 방법들로 충분하지 않을 때 사용합니다.
5. **`as` 캐스팅.** 오직 검증 후에만 사용합니다.

```ts
function area(s: Shape): number {
  if ("radius" in s) return Math.PI * s.radius ** 2; // narrowed to circle
  return s.width * s.height; // narrowed to rect
}
```

## 타입 가드 (Type guards)

가드는 주장을 실제로 검증해야 합니다. 거짓말을 하는 가드는 `as`보다 더 나쁩니다.

```ts
function isCircle(s: Shape): s is Shape & { kind: "circle" } {
  return s.kind === "circle";
}
```

가능한 경우 판별자를 통한 타입 좁히기를 우선하세요.

## 완전성 검사 (Exhaustiveness)

default 분기에서는 판별 대상을 `never` 타입 지역 변수에 할당하세요.

```ts
// Value-returning switch
function area(s: Shape): number {
  switch (s.kind) {
    case "circle":
      return Math.PI * s.radius ** 2;
    case "rect":
      return s.width * s.height;
    default: {
      const _exhaustive: never = s;
      return _exhaustive;
    }
  }
}

// Void switch
function handle(s: Shape): void {
  switch (s.kind) {
    case "circle":
      drawCircle(s);
      break;
    case "rect":
      drawRect(s);
      break;
    default: {
      const _exhaustive: never = s;
      void _exhaustive;
    }
  }
}
```

값을 반환하는 switch에서는 return 스타일을, 문장(statement)을 실행하는 switch에서는 void 스타일을 사용합니다.

## `as` 대신 `satisfies` (`satisfies` over `as`)

`satisfies`는 리터럴 타입을 넓히지 않으면서 값을 검증합니다.

```ts
// Don't. Widens, loses literal types.
const config = { theme: "dark", cols: 3 } as Config;

// Do. Validates AND preserves literal types.
const config = { theme: "dark", cols: 3 } satisfies Config;
// config.theme is "dark" (literal), not string
```

## 경계 검증 (Boundary validation)

데이터가 들어오는 경계에서 한 번만 검증하고 내부에서는 타입을 신뢰하세요. **boundary-discipline** 원칙 스킬을 참조하세요.

- **와이어 포맷 (Wire formats)** (proto, JSON-RPC): 향후 호환 가능한 변경 사항이 구버전 클라이언트를 깨뜨리지 않도록 `ignoreUnknownFields`로 파싱하세요.
- **영속화된 JSON (Persisted JSON):** 파싱 주변을 try/catch로 감싼 버전 관리되는 블롭(versioned blob)으로 다루세요.
- 호출 체인 깊숙한 곳에서 **중복 검증하지 마세요.**

## 스키마 파생 타입 (Schema-derived types)

`.proto`, OpenAPI 명세, GraphQL 스키마, 또는 데이터베이스 마이그레이션이 이미 형태를 정의하고 있다면, 타입을 중복 선언하지 말고 생성된 타입에서 파생시키세요.

```ts
// Don't. Duplicate shape, drifts when the schema changes.
type CheckSummary = {
  totalCount: number;
  checks: { name: string; status: string }[];
};
function renderChecks(s: CheckSummary) {
  /* ... */
}

// Do. Derive from the generated schema type.
import type { ChecksMessage } from "<generated module>";
function renderChecks(s: Pick<ChecksMessage, "totalCount" | "checks">) {
  /* ... */
}
```

새 인터페이스를 작성하기 전에 `Pick`, `Omit`, `Parameters`, `ReturnType`, `Awaited`, `typeof`를 먼저 활용하세요.

## 객체 인자 (Object args)

```ts
// Don't. Swap two args, still compiles.
openFile(uri, {
  startLineNumber: 10,
  startColumn: 1,
  endLineNumber: 10,
  endColumn: 1,
});

// Do. Order-independent, self-documenting.
openFile({
  uri,
  selection: {
    startLineNumber: 10,
    startColumn: 1,
    endLineNumber: 10,
    endColumn: 1,
  },
});
```

프레임당 렌더링, 토크나이저, 파서 등 타이트한 루프 안에서 객체 할당 비용이 문제가 되는 핫 패스(hot paths)에서는 건너뛰세요.

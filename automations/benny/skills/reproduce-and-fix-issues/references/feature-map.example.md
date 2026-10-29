# 기능 맵 예시 (Feature-map example)

Benny가 재현할 수 있는 모든 사용자 대면(user-facing) 기능을 매핑하세요. 앱을 구동하기 전에 관련 섹션을 읽으세요. 이 맵은 사용자 관점으로 유지하세요. 내부 구현과 현재 코드 경로는 여기에 고정해 두는 대신 런타임에 탐색하세요.

이 파일을 `.cursor/automations/benny/` 바깥(예: `.cursor/benny/feature-map.md`)으로 복사하고 `control.feature_map_path`가 그 복사본을 가리키게 하세요. 팩을 새로고침하더라도 이 파일이 덮어써져서는 안 됩니다.

## 기능별 템플릿 (Per-feature template)

### `<feature name>`

`<one-line user-visible purpose>`

#### 사용자가 도달하는 방법 (How a user gets there)

- 클릭 경로: `<screen> -> <menu, tab, or panel> -> <control>`
- 키보드 단축키: `<shortcut or none>`

#### 컨트롤 어댑터가 구동하는 방법 (How the control adapter drives it)

- `<inputs>`와 함께 `<adapter action>`을 실행하면 `<visible result>`가 나타나야 합니다.
- 초기화(Reset): `<how the adapter returns to a fresh state>`.

#### 안정적인 셀렉터 (Stable selectors)

- `<role and accessible name>`
- `<ARIA relationship>`
- `<data-component or purpose-named data attribute>`

생성된 CSS 또는 StyleX 클래스, 동적 해시, 자식 인덱스 또는 깨지기 쉬운 DOM 위치는 절대 사용하지 마세요.

#### 실행해 볼 상태 (States to exercise)

- Default, hover, focus-visible, active, disabled
- Loading, empty, error
- Selected, open, expanded
- `<relevant feature-specific variants>`

해당하지 않는 상태는 표시해 두세요.

#### 사전 조건 및 셋업 (Preconditions and setup)

- 인증(Auth): `<account state>`
- 데이터(Data): `<fixture>`
- 권한(Permissions): `<role>`
- 플래그(Flags): `<flag or none>`
- 서비스(Services): `<required availability>`

#### 증거 및 교차 검증 (Evidence and cross-check)

- 스크린샷: `<app identity, feature, and discriminating state>`
- 비디오: `<entry path, interaction, and final state>`
- 교차 검증(Cross-check): `<read-only state or value that confirms the UI>`

#### 주의 사항 (Gotchas)

- `<known dead end or wrong surface>`
- `<safe environment translation>`

## 가상의 예시 (Fictional example)

다음 기능들은 가상의 할 일 관리(task) 앱에 속한 것입니다. 이는 예시일 뿐이며 필수 Benny 기능이 아닙니다.

### Sign in

사용자가 할 일 관리 앱에 들어갈 수 있게 합니다.

#### How a user gets there

- 앱을 열고 `Sign in`을 선택합니다. 단축키는 없습니다.

#### How the control adapter drives it

- `open_app`, `click Sign in`, `fill credentials`, `click Continue`를 실행하면 항목 목록(item list)이 열려야 합니다.
- 로그아웃하고 일회용 세션을 지우는 것으로 초기화합니다.

#### Stable selectors

- 버튼 `Sign in`, 텍스트박스 `Email` 및 `Password`, `data-component="sign-in-form"`

#### States to exercise

- Default, focus-visible, submitting, disabled, loading, error

#### Preconditions and setup

- 일회용 계정 및 가용한 인증 서비스

#### Evidence and cross-check

- 랜딩 페이지부터 항목 목록까지 녹화합니다. 읽기 전용 세션 상태를 확인합니다.

#### Gotchas

- 마케팅 페이지는 잘못된 화면(surface)입니다. 인증 서비스 누락은 차단(block) 요인입니다.

### Item list and detail

사용자가 항목을 둘러보고 하나를 열 수 있게 합니다.

#### How a user gets there

- `Items` 탭을 연 다음 행(row)을 선택합니다.

#### How the control adapter drives it

- `select_tab Items`와 `click <fixture item>`을 실행하면 해당 상세 화면이 열려야 합니다.
- 상세 화면을 닫고 선택을 해제하는 것으로 초기화합니다.

#### Stable selectors

- `Items`라는 이름의 탭과 리스트, 픽스처 이름의 행, `data-component="item-detail"`

#### States to exercise

- Loading, empty, error, selected, open, expanded

#### Preconditions and setup

- 명명된 픽스처 항목들, 읽기 권한, 가용한 항목 서비스

#### Evidence and cross-check

- 선택 상태와 일치하는 상세 제목을 보여줍니다. 선택된 항목 ID를 확인합니다.

#### Gotchas

- 검색 결과는 비슷해 보일 수 있지만 다른 경로를 사용합니다.

### Item editor

사용자가 항목을 생성하거나 편집할 수 있게 합니다.

#### How a user gets there

- 상세 화면에서 `Edit`를 선택하거나 목록에서 `New item`을 선택합니다.

#### How the control adapter drives it

- `click Edit`, `fill <field>`, `click Save`를 실행하면 상세 화면이 업데이트되어야 합니다.
- 픽스처를 복원하는 것으로 초기화합니다.

#### Stable selectors

- 버튼 `Edit`, `New item`, `Save`, 폼 `Item editor`, 레이블로 연결된 필드들

#### States to exercise

- Default, focus-visible, dirty, validating, disabled, saving, error, success

#### Preconditions and setup

- 편집 가능한 픽스처, 쓰기 권한, 가용한 저장 서비스

#### Evidence and cross-check

- 업데이트된 상세 화면을 통해 필드 변경을 보여줍니다. 저장된 항목 값을 읽기 전용으로 확인합니다.

#### Gotchas

- 폼 상태를 직접 주입하지 마세요. 읽기 전용 상세 필드는 에디터가 아닙니다.

### Settings

사용자가 개인 환경설정을 변경할 수 있게 합니다.

#### How a user gets there

- 프로필 메뉴를 연 다음 `Settings`를 선택합니다.

#### How the control adapter drives it

- `open_menu Profile`, `click Settings`, `toggle <preference>`를 실행하면 컨트롤이 업데이트되어야 합니다.
- 시작 시점의 환경설정을 복원하는 것으로 초기화합니다.

#### Stable selectors

- 버튼 `Profile`, 메뉴 항목 `Settings`, 리전(region) `Settings`, 목적에 따라 명명된 환경설정 속성

#### States to exercise

- Closed, open, selected, focus-visible, disabled, loading, error

#### Preconditions and setup

- 로그인된 테스트 계정, 알려진 환경설정 값, 가용한 환경설정 서비스

#### Evidence and cross-check

- 메뉴 경로와 최종 컨트롤 상태를 보여줍니다. 환경설정 값을 읽기 전용으로 확인합니다.

#### Gotchas

- 운영체제 설정은 다른 화면(surface)입니다.

## 완전성 체크리스트 (Completeness checklist)

- 재현 가능한 모든 사용자 대면 기능에 섹션이 있습니다.
- 모든 섹션이 사용자 경로, 어댑터 액션, 초기화 방법을 명시합니다.
- 셀렉터는 역할, 이름, ARIA, 안정적인 컴포넌트 마커 또는 목적에 따라 명명된 속성을 사용합니다.
- 생성된 클래스나 DOM 위치를 사용하는 셀렉터가 없습니다.
- 관련된 상호작용, 로딩, 빈 상태, 에러, 선택됨, 펼쳐짐 상태가 다루어집니다.
- 인증, 픽스처, 권한, 플래그, 서비스가 명시적입니다.
- 스크린샷, 비디오 및 기저 교차 검증 요구사항이 명시적입니다.
- 잘못된 화면, 막다른 경로(dead ends), 안전한 환경 변환이 나열되어 있습니다.
- 세부 구현 사항은 런타임에 탐색할 영역으로 남겨 둡니다.

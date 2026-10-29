# Notes 검증 맵 (Notes verification map)

이 디렉터리는 Notes 앱의 사용자 대면 동작을 검증하기 위해 유지관리되는 정본 소스입니다. 앱을 구동하기 전에 이 인덱스를 먼저 읽고, 해당하는 기능 파일을 레시피로 사용하세요.

## 기본 전제 조건 (Baseline preconditions)

- 일회용 데이터 디렉터리를 사용하여 `http://127.0.0.1:4173`에서 Notes를 실행합니다.
- 동시 실행 간에 상태가 공유되지 않도록 `NOTES_DATA_DIR=/tmp/notes-verify-$RUN_ID`를 설정합니다.
- 제목이 `Quarterly plan`과 `Grocery list`인 노트를 시드 데이터로 넣습니다.
- `control-notes`와 `notes` CLI를 `PATH`에 추가합니다.
- `control-notes doctor`를 실행하여 예상 URL, 데이터 디렉터리, 빌드 리비전이 일치하는지 확인합니다.
- 이번 검증 실행이 직접 시작하지 않은 인스턴스는 절대 구동하지 마세요.

## 구동 관례 (Driving conventions)

- 전제 조건에서 달리 명시하지 않는 한 모든 레시피는 기본 상태(baseline state)에서 시작합니다.
- CSS 셀렉터나 DOM 위치보다 ARIA 역할(role)과 접근 가능한 이름(accessible name)을 우선 사용합니다.
- 모든 명령어는 문자 그대로(literal) 취급합니다. 따옴표로 묶인 이름과 플래그를 변경하지 마세요.
- 브라우저 액션은 `control-notes browser`를 통해 실행합니다.
- 터미널 액션은 `control-notes cli -- <command>`를 통해 실행합니다.
- 상태 변경(mutation) 후에는 시드 데이터를 원래대로 복원합니다. 단, 정리(cleanup) 과정에서 증명 아티팩트를 삭제하지 마세요.

## 증명 및 건너뛰기 보고 (Proof and skip reporting)

- 최종 화면뿐만 아니라 사용자의 액션과 그로 인한 결과 상태를 함께 캡처합니다.
- UI 증명에는 ARIA 스냅샷과 앱 식별 정보가 보이는 스크린샷이 포함됩니다.
- CLI 증명에는 실행한 명령어, stdout, stderr, 종료 코드(exit code)가 포함됩니다.
- 상태 변경 증명에는 저장된 값을 확인하는 두 번째 읽기 전용 뷰가 포함됩니다.
- 모든 아티팩트에 기능 ID와 사용한 진입점을 함께 기록합니다.
- 도달할 수 없는 경로는 시도한 명령어 및 충족되지 않은 전제 조건과 함께 보고합니다.
- 건너뛴 진입점을 다른 경로를 통해 검증 완료된 것으로 보고하지 마세요.

## 기능 항목 계약 (Feature entry contract)

각 기능 파일은 H1 제목과 사용자에게 보이는 동작을 설명하는 한 단락으로 시작합니다. 그런 다음 반드시 아래 순서대로 정확히 네 개의 H2 섹션을 사용합니다.

1. `Sub-features`: 각 동작을 한 줄씩 설명하는 짧은 ID 목록을 나열합니다.
2. `How to get to it (user POV)`: 모든 사용자 진입점을 나열합니다.
3. `Driving it with <harness>`: `Preconditions:`로 시작하며, 각 사용자 액션을 정확한 명령어 및 관찰 가능한 결과와 짝지은 라벨 불릿들을 사용합니다.
4. `Gotchas`: 검증 실행을 낭비하게 만들거나 무효화할 수 있는 함정들을 나열합니다.

구현 세부사항은 맵에 넣지 마세요. 오직 사용자 경로, 안정적인 핸들, 필수 상태, 명령어, 관찰 가능한 증명만 명시하세요.

## 기능 목록 (Features)

- [Create a note](./create-note.md): 브라우저 및 CLI에서의 노트 생성, 취소, 영속성 확인, 정리를 다룹니다.
- [Search notes](./search.md): 툴바, 키보드, CLI를 통한 검색과 일치, 빈 결과, 검색어 지우기 상태를 다룹니다.

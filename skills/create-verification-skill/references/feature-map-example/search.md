# 노트 검색 (Search notes)

검색 기능은 사용자가 제목이나 본문 텍스트로 노트를 찾고, 일치하는 노트를 확인하며, 일치 항목이 없는 상태와 검색 기능을 사용할 수 없는 상태를 구분할 수 있게 해줍니다.

## Sub-features

- `search-open`: 지원되는 각 브라우저 진입점에서 검색창을 엽니다.
- `search-match`: 노트 데이터를 변경하지 않고 제목 및 본문 일치 결과를 반환합니다.
- `search-open-result`: 검색 결과를 노트 에디터에서 엽니다.
- `search-empty`: 일치하는 항목이 없는 검색어에 대해 완전한 빈 상태(empty state)를 보여줍니다.
- `search-clear`: 검색어를 지우고 최근 노트 뷰를 복원합니다.
- `search-cli`: 터미널에서 동일한 일치 노트들을 반환합니다.

## How to get to it (user POV)

- 브라우저 툴바에서 `Search` 버튼을 선택합니다.
- 포커스가 편집 가능한 필드 밖에 있을 때 브라우저에서 `/` 키를 누릅니다.
- 터미널에서 `notes search <query>`를 실행합니다.

## Driving it with control-notes

Preconditions:

- Notes가 `http://127.0.0.1:4173`에서 정상 실행 중입니다.
- 일회용 데이터 디렉터리에 본문 텍스트가 `Draft budget`인 `Quarterly plan` 노트가 들어 있습니다.
- `control-notes doctor`가 예상 URL과 데이터 디렉터리를 보고합니다.

- **툴바 진입점 (Toolbar entry).** `Search` 버튼을 선택합니다. `control-notes browser click --role button --name "Search"`를 실행합니다. 검색박스에 포커스가 맞춰진 `Search notes` 다이얼로그가 나타납니다.
- **키보드 진입점 (Keyboard entry).** 다이얼로그를 닫고 페이지에 포커스를 맞춘 뒤 `/` 키를 누릅니다. `control-notes browser press --key "/"`를 실행합니다. 동일한 다이얼로그가 나타나며 페이지에 슬래시 문자가 입력되지 않습니다.
- **제목 일치 (Title match).** `quarterly`를 입력합니다. `control-notes browser fill --role searchbox --name "Search notes" --value "quarterly"`를 실행합니다. `Search results` 목록에 `Quarterly plan`이 포함되며 `Grocery list`는 포함되지 않습니다.
- **본문 일치 (Body match).** 검색어를 `budget`으로 바꿉니다. `control-notes browser fill --role searchbox --name "Search notes" --value "budget"`을 실행합니다. 본문 일치 발췌문과 함께 `Quarterly plan` 결과가 계속 표시됩니다.
- **결과 열기 (Open result).** `Quarterly plan`을 선택합니다. `control-notes browser click --role link --name "Quarterly plan"`을 실행합니다. 다이얼로그가 닫히고 에디터 헤딩이 `Quarterly plan`으로 표시됩니다.
- **빈 상태 (Empty state).** 검색창을 다시 열고 `volcano`를 입력합니다. `control-notes browser fill --role searchbox --name "Search notes" --value "volcano"`를 실행합니다. 검색이 완료된 후 `No matching notes` 상태가 나타납니다.
- **검색어 지우기 (Clear query).** `Clear search`를 선택합니다. `control-notes browser click --role button --name "Clear search"`를 실행합니다. 검색박스가 비워지고 결과 목록 대신 `Recent notes` 영역이 다시 표시됩니다.
- **CLI 일치 (CLI match).** 터미널에서 검색합니다. `control-notes cli -- notes search "quarterly" --format json`을 실행합니다. 종료 코드 `0`과 함께 stdout에 제목이 `Quarterly plan`인 객체 하나가 포함됩니다.
- **CLI 불일치 (CLI miss).** 존재하지 않는 값을 검색합니다. `control-notes cli -- notes search "volcano" --format json`을 실행합니다. 종료 코드 `0`과 함께 stdout은 `[]`가 됩니다.
- **증명 (Proof).** 결과가 채워진 상태를 캡처합니다. `control-notes browser snapshot --aria --path artifacts/search/results.aria.txt`와 `control-notes browser screenshot --path artifacts/search/results.png`를 실행합니다. 두 아티팩트 모두 Notes 앱, 검색어, 그리고 `Quarterly plan`을 식별할 수 있게 보여줍니다.

## Gotchas

- 에디터나 검색박스에 포커스가 있을 때 `/`를 누르면 검색창이 열리는 대신 텍스트가 입력됩니다.
- 검색 결과는 짧은 디바운스(debounce) 후에 업데이트됩니다. 고정된 시간(sleep) 동안 기다리지 말고 결과 목록이나 빈 상태 메시지가 나타날 때까지 대기하세요.
- 사용자가 `Include archived`를 활성화하지 않는 한 보관된(archived) 노트는 검색에서 제외됩니다.
- CLI는 기본적으로 사람이 읽기 좋은 형식을 출력합니다. 안정적인 단언(assertion)을 위해 `--format json`을 사용하세요.
- 검색 결과를 열면 브라우저 상태가 바뀝니다. 다른 검색어를 증명하기 전에 검색창을 다시 여세요.

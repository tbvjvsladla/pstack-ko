# 노트 생성 (Create a note)

노트 생성 기능은 사용자가 브라우저나 CLI에서 제목이 있는 노트를 저장하고, 작성 중인 초안을 취소하며, 두 번째 사용자 대면 뷰에서 저장된 노트를 확인할 수 있게 해줍니다.

## Sub-features

- `create-open`: 각 브라우저 진입점에서 빈 에디터를 엽니다.
- `create-save`: 제목과 본문을 영구 저장합니다.
- `create-cancel`: 작성 중인 브라우저 초안을 폐기합니다.
- `create-cli`: 터미널에서 동일한 형태의 노트를 생성합니다.

## How to get to it (user POV)

- 브라우저 툴바에서 `New note` 버튼을 선택합니다.
- 포커스가 편집 가능한 필드 밖에 있을 때 브라우저에서 `n` 키를 누릅니다.
- 터미널에서 `notes create --title <title> --body <body>`를 실행합니다.

## Driving it with control-notes

Preconditions:

- Notes가 `http://127.0.0.1:4173`에서 정상 실행 중입니다.
- `Release checklist`라는 제목의 노트가 존재하지 않습니다.
- `control-notes doctor`가 예상 URL과 일회용 데이터 디렉터리를 보고합니다.

- **에디터 열기 (Open editor).** `New note`를 선택합니다. `control-notes browser click --role button --name "New note"`를 실행합니다. `Title` 텍스트박스에 포커스가 맞춰진 `Note editor` 폼이 나타납니다.
- **내용 입력 (Enter content).** 제목과 본문을 입력합니다. `control-notes browser fill --role textbox --name "Title" --value "Release checklist"`와 `control-notes browser fill --role textbox --name "Body" --value "Tag and publish"`를 실행합니다. `Save note` 버튼이 활성화됩니다.
- **노트 저장 (Save note).** `Save note`를 선택합니다. `control-notes browser click --role button --name "Save note"`를 실행합니다. `Note saved` 상태 메시지가 나타나고 헤딩이 `Release checklist`로 표시됩니다.
- **영속성 확인 (Confirm persistence).** 노트 목록으로 돌아가 해당 노트를 다시 엽니다. `control-notes browser click --role link --name "All notes"`와 `control-notes browser click --role link --name "Release checklist"`를 실행합니다. 에디터에 저장된 두 값이 모두 표시됩니다.
- **초안 취소 (Cancel draft).** 새 노트를 열고 `Discard me`를 입력한 뒤 `Cancel`을 선택합니다. `control-notes browser click --role button --name "New note"`, `control-notes browser fill --role textbox --name "Title" --value "Discard me"`, `control-notes browser click --role button --name "Cancel"`을 실행합니다. 노트 목록으로 돌아오며 `Discard me` 링크가 존재하지 않습니다.
- **CLI 진입점 (CLI entry).** 두 번째 노트를 생성합니다. `control-notes cli -- notes create --title "CLI note" --body "Created from terminal" --format json`을 실행합니다. 종료 코드 `0`과 함께 stdout에 새 노트 ID와 제목이 포함됩니다.
- **증명 (Proof).** `All notes`에서 저장된 두 노트를 모두 확인합니다. `control-notes browser snapshot --aria --path artifacts/create-note/list.aria.txt`와 `control-notes browser screenshot --path artifacts/create-note/list.png`를 실행합니다. 아티팩트에 `Release checklist`와 `CLI note`가 표시됩니다.

## Gotchas

- 텍스트박스에 포커스가 있을 때 `n`을 누르면 새 에디터가 열리는 대신 문자가 입력됩니다.
- 저장 시 제목 앞뒤 공백이 제거(trim)됩니다. 초안 입력값이 아니라 렌더링된 제목을 단언(assert)하세요.
- 저장 상태 메시지만으로는 충분한 증명이 되지 않습니다. 반드시 목록에서 노트를 다시 열어 확인하세요.
- 픽스처 정리(fixture cleanup) 중에 `Release checklist`와 `CLI note`는 제거하되, 증명 아티팩트는 그대로 보존하세요.

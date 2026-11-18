### PR 열기 (Opening a PR)

다른 모든 플레이북의 마지막에 호출됩니다.

트렁크에서 분기한 git 워크트리에서 작업하고, 무관한 작업은 섞지 마세요. 작고 반영 가능한(landable) 커밋들로 리베이스하세요. 커밋하기 전에 diff에 대해 `cursor-team-kit`의 `/deslop`을 실행하세요.

제목: `type(scope): subject`. 특정 심볼이 변경의 핵심을 담당할 때는 그 실제 심볼 이름을 명시하세요. 본문: 40줄 이내의 1분 브리핑으로, 아래의 `##` 헤딩들을 순서대로 사용하고 비어 있는 섹션은 생략하세요: `## Why`, `## What changed`, `## Scope`, `## Tradeoffs` (리뷰어가 그렇지 않으면 물어볼 법한 기각된 대안들만), `## Blast Radius`, `## Verification`.

후속 작업은 베이스 브랜치로 스택을 쌓으세요. 실행 환경의 PR 도구에서 `draft: false`를 지정해 즉시 리뷰 가능한(ready) 상태로 PR을 여세요. PR 도구가 없다면 `--draft` 없이 `gh pr create`를 사용하거나, Origin CLI가 저장소를 확인할 수 있을 때는 `origin pr create --status open`을 사용하세요. PR을 열었다고 해서 베이비시팅이 시작되지는 않습니다.

# 코드 고고학 (Code Archaeology: git + 저장소 내부)

## 이 소스에 담긴 내용 (What this source contains)

- 커밋 히스토리 (메시지, 날짜, 작성자, diff)
- PR 설명, 리뷰 댓글, 토론 스레드 (`gh`를 통해 조회)
- 인라인 코드 주석, TODO, FIXME, 지원 중단(deprecation) 안내
- 저장소에서 관리하는 경우 ADR (아키텍처 결정 기록, architectural decision records)
- 테스트 코드 (테스트 이름과 단언문(assertions)은 변경의 동기가 된 엣지 케이스를 담고 있는 경우가 많음)
- 동일한 커밋에서 함께 수정된 관련 파일들 (동시 변경 신호, co-change signal)
- 저장소 내의 CHANGELOG 항목 및 릴리스 노트
- 커밋 메시지와 PR 본문에 적힌 이슈/티켓 ID

코드와 직접 연결되어 있어 가장 신뢰할 수 있고 가장 완전한 소스입니다. 저장소를 거쳐 간 모든 변경의 기록이 여기에 남아 있어야 합니다.

## 검색 방법 (How to search it)

초기 시드(seed) 커밋 목록을 확장하세요:

```bash
# 파일 이름 변경을 추적하며 히스토리 조회
git log --follow --oneline -- <file>

# Pickaxe: 특정 문자열을 추가하거나 삭제한 커밋 검색
git log -S '<exact_string_from_code>' -- <file>

# 또는 정규식 패턴으로 검색:
git log -G '<regex>' -- <file>

# 각 줄을 누가 언제 작성했는지 확인
git blame -L <start>,<end> <file>

# 단일 커밋의 전체 diff 조회
git show <hash>

# 두 시점 사이에 이 파일에 영향을 준 커밋들 조회
git log <old>..<new> -p -- <file>
```

코드 외적인 문서(out-of-band docs)를 찾아보세요:

```bash
# ADR은 주로 docs/adr/ 등에 위치함
rg -l -i 'architecture.decision' --glob '*.md'

# 대상 코드 주변의 TODO 및 FIXME 검색
rg -n -C2 '(TODO|FIXME|HACK|XXX|NOTE)' <target_file>

# 관련 테스트 검색
rg -l '<symbol>' --glob '*test*'
```

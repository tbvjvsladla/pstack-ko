# Sentry 에러 히스토리 (Sentry Error History)

## 검색 방법 (How to search it)

Sentry MCP를 사용하세요.

1. **현황 파악하기 (Orient).** 프로젝트 슬러그와 조직을 모른다면:

   ```
   find_organizations
   find_projects
   ```

2. **대상 코드와 관련된 이슈 검색하기.**

   ```
   search_issues (자연어 쿼리, 예: "errors in PaymentService timeout", "unhandled exceptions in uploadFile")
   ```

   대상 코드가 처리하는 예외 클래스 이름, 함수 또는 클래스 이름, 코드가 검사하는 에러 메시지 문자열, 그리고 파일 경로를 조합해 쿼리를 만드세요.

3. **릴리스와 시간 창으로 범위 좁히기.**

   ```
   search_issue_events (릴리스, 시간, 환경, 트레이스 ID, 태그로 필터링)
   get_issue_tag_values (특정 이슈에 대해 버전, 사용자, 환경별 분포 확인)
   ```

   의심되는 이슈에 대해 **최초 관측(first seen)** 및 **최종 관측(last seen)** 시점(에러가 멈춘 시점이 대상 코드의 배포 날짜와 일치하는가?), **영향받은 릴리스**(어떤 릴리스에서 발생했고 어떤 릴리스에서 수정되었는가?), 그리고 **빈도 궤적**(스파이크가 발생한 뒤 해결되었는가?)을 확인하세요.

4. **맥락 파악을 위해 전체 이벤트 가져오기.**

   ```
   get_sentry_resource (Sentry URL 또는 type+ID 전달)
   ```

   스택 트레이스가 대상 코드를 통과하는가? 태그와 브레드크럼(breadcrumbs)이 대상 코드가 방어하려는 조건과 일치하는가?

5. **대상 코드 무렵에 배포된 릴리스 확인하기.**

   ```
   find_releases (대상 코드의 커밋 날짜 전후)
   ```

   릴리스 버전을 PR의 머지 날짜와 교차 검증하세요.

6. **Seer는 제한적으로만 사용하기.**

   ```
   analyze_issue_with_seer
   ```

   Seer의 AI 근본 원인 분석은 가설 생성기일 뿐입니다. 이를 권위 있는 사실이 아닌 추론(inference)으로 취급하세요. 실제 이벤트와 스택 트레이스가 일차적인 근거입니다.

## 반환할 내용 (What to return)

관련된 각 이슈에 대해:
- 이슈 ID 및 제목
- 프로젝트 및 조직
- 최초 관측(First seen) / 최종 관측(Last seen) 타임스탬프
- 이벤트 카운트 (알고 있는 경우 샘플링 비율 포함)
- 영향받은 릴리스
- 대상 코드와의 관련성을 보여주는 대표적인 스택 트레이스 스니펫 (요약이 아닌 원문 발췌)
- 대상 코드의 배포 날짜와 최초/최종 관측 시점 간의 상관관계
- 이슈 링크
- 작성자 댓글이나 해결 노트(resolution notes)

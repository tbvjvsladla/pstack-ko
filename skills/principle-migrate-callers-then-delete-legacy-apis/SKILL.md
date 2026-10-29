---
name: principle-migrate-callers-then-delete-legacy-apis
description: >-
  기존 호출자들이 여전히 존재하는 상태에서 새로운 내부 API를 도입할 때 적용합니다. 호환성 계층을 유지하는 대신 같은
  작업 단위(wave) 안에서 호출자들을 마이그레이션하고 구버전 API를 삭제하세요.
disable-model-invocation: true
---

# 호출자를 마이그레이션한 뒤 레거시 API를 삭제하라 (Migrate Callers Then Delete Legacy APIs)

- 새로운 계약을 검증하도록 테스트를 업데이트하고, 리팩터링 이전의 내부 구현 세부사항만 보호하던 테스트는 삭제하세요

**적용 조건:**
- 하위 호환성에 의존하는 외부 사용자가 없을 때
- 프로젝트가 조율된 호환성 파괴 변경(breaking changes)을 수용할 수 있을 때
- 새 API가 단순화 또는 리팩터링 이니셔티브의 일환일 때

# Databricks 분석 및 시스템 테이블 (Databricks Analytics & System Tables)

## 이 소스에 담긴 내용 (What this source contains)

Databricks는 제품 분석, 데이터 파이프라인, 그리고 웨어하우스 텔레메트리 계층입니다. 이는 Datadog을 보완합니다. Datadog이 *인프라/런타임* 관점이라면, Databricks는 *제품/데이터* 관점(사용자가 무엇을 했는지, 어떤 실험이 실행되었는지, 기능 사용량이 어떻게 변화했는지, 임계값 상수가 어디서 유래했는지)을 보여줍니다.

- **제품 분석 이벤트 (Product analytics events).** `your_warehouse.events.analytics_track_event`(원시 테이블) 및 `<your_analytics_db>.<schema>.<table>`에 있는 타입이 지정되고 중복 제거된 이벤트별 dbt 모델. 사용자 행동: 기능 호출, 클릭, 수락/거절, 제출, 클라이언트가 보고한 에러.
- **사용량 및 과금 이벤트 (Usage & billing events).** `your_warehouse.events.usage_event` / `<your_analytics_db>.<schema>.stg_usage_events`, `your_warehouse.events.raw_model_event` / `<your_analytics_db>.<schema>.stg_raw_model_events`. 비용이나 볼륨에 의해 주도된 의사결정을 파악할 때 유용합니다.
- **실험 / 피처 플래그 데이터 (Experiment / feature-flag data).** 노출(exposure) 및 결과(outcome) 테이블. **스키마는 회사마다 다릅니다.** 테이블 이름을 단정하기 전에 `SHOW TABLES`로 먼저 확인하세요.
- **시스템 테이블 (System tables).** `system.query.history`, `system.compute.warehouses`, `system.billing.*`, `system.access.audit`. "이 쿼리가 비용이 많이 들었나?", "이걸 얼마나 자주 실행했나?", "웨어하우스 부하가 언제 치솟았나?" 같은 질문에 답합니다.
- **dbt 리니지 (dbt lineage).** `<your_analytics_db>.<schema>`의 모델들은 어떤 파이프라인이 특정 테이블/필드에 의존하는지 보여줍니다. 업스트림 변경은 소비자 코드(consumer-code) 변경의 동기가 되는 경우가 많습니다.
- **Databricks 노트북 (Databricks notebooks).** 엔지니어들이 코드 변경 전에 작성한 탐색적 분석. **SQL MCP를 통해서는 쿼리할 수 없습니다.** 설계 근거가 노트북에 있다고 의심된다면 이를 공백(gap)으로 명시하세요.

## 검색 방법 (How to search it)

Databricks SQL MCP를 사용하세요. 주요 도구: `execute_sql_read_only`. 만약 `statement_id`를 반환하면 쿼리를 다시 실행하지 말고 `poll_sql_result`로 폴링하세요.

**쿼리하기 전에 구조를 먼저 파악하세요.** 스키마는 회사마다 다릅니다. 테이블 이름을 신뢰하기 전에 먼저 탐색하세요:

```sql
SHOW TABLES IN <your_analytics_db>.<schema> LIKE '*<keyword>*';
DESCRIBE TABLE <your_analytics_db>.<schema>.stg_<event>;
```

**모든 쿼리에 시간 범위를 지정하세요.** 이 테이블들은 매우 거대해서 시간 제약 없는 스캔은 타임아웃이 발생합니다. 배포 날짜(ship date) 전후를 감싸는 시간 창(보통 전후 ~30일, 타당한 이유가 있을 때만 더 넓게)으로 `_timestamp`(이벤트) 또는 `start_time`(`system.query.history`)을 필터링하세요.

**원시 테이블보다 타입이 지정된 dbt 모델을 우선하세요.** `<your_analytics_db>.<schema>.<table>`은 중복이 제거되고 타입이 지정되어 있으며 액체 클러스터링(liquid-clustered)되어 있습니다. 반면 `your_warehouse.events.analytics_track_event`에는 중복 데이터와 타입이 없는 `properties_json`이 들어 있습니다. 모델 이름 패턴은 `stg_<source>_<event_name_with_underscores>`이며, 여기서 `<source>`는 `app`, `backend`, `website`, 또는 `cli`입니다. 패턴만으로 확실하지 않을 때는 `SHOW TABLES`로 정확한 모델 이름을 확인하세요. 아직 dbt 모델이 없거나 dbt 갱신 지연 시간(refresh lag) 이내의 최신 이벤트가 필요한 경우에만 원시 테이블로 내려가세요.

**타입이 지정된 dbt 모델의 컬럼 규칙** (이를 알아두면 `DESCRIBE` 왕복 호출을 줄일 수 있습니다):

- `_timestamp`, `_id`, `_auth_id`, `_request_id`, `event_name`. 모든 모델의 표준 컬럼
- `properties_<name>`. 타입이 지정되고 언더스코어 표기법(underscore-cased)이 적용된 이벤트 속성 (`properties_entrypoint`, `properties_size_bytes`, …)
- `context_team_id`, `context_client_version`, `context_country`, `context_client_os`. 사전 추출된 클라이언트 컨텍스트

### 효과를 보기 좋은 조사 패턴 (Investigation patterns that tend to pay off)

조사 대상에 맞는 테이블 + 컬럼 조합을 선택하세요:

1. **이벤트 사용량 궤적 (Event usage trajectory).** PR 머지 전후 ±30일 창에 걸친 관련 `stg_*` 모델의 일별 카운트. 머지 후 하루이틀 내에 0에서 꾸준한 볼륨으로 계단 함수(step function) 형태의 상승이 나타난다면, 해당 PR이 그 기능을 출시했다는 강력한 정황 근거입니다. 반대로 0으로 감소한다면 지원 중단(deprecation)이나 삭제를 시사합니다.
2. **가드레일 / 방어적 체크의 기원 (Guard-rail / defensive-check origin).** PR *이전* 14일 동안 관련 `properties_<name>` 컬럼의 분포(중앙값 / p99 / 최댓값). p99 값이 대상 코드의 임계값 상수와 일치한다면 그 숫자가 데이터로부터 선택되었음을 시사합니다.
3. **실험 / 피처 플래그 조회 (Experiment / feature-flag lookup).** `SHOW TABLES ... LIKE '*experiment*'`로 노출 테이블을 찾은 뒤, PR 날짜 무렵 해당 플래그 키의 변형(variant)별 노출 횟수를 조회하세요.
4. **마이그레이션, 백필, 성능 재작성에 대한 쿼리 히스토리 근거.** 좁은 `start_time` 창과 함께 `statement_text ILIKE '%<table_or_symbol>%'`로 필터링한 `system.query.history`는 해당 변경의 동기가 되었을 법한 고비용 쿼리들을 드러냅니다(`total_duration_ms`로 정렬하거나 `SUM(read_bytes)`, `COUNT(*)`로 집계).
5. **dbt 리니지 (dbt lineage).** 대상 코드가 `<your_analytics_db>.<schema>` 모델을 읽거나 쓰는 경우, 해당 모델 자체의 git 히스토리(이 저장소 내)에 설계 근거가 담겨 있는 경우가 많습니다. 직접 추적하지 말고 그 단서를 git 조사자에게 넘기세요.

## 좋은 근거의 모습 (What good evidence looks like here)

위 패턴 형태 외에도 다음과 같은 것들이 좋은 근거가 됩니다:

- 방어적 코드 PR 이후 며칠 만에 특정 에러 분류 이벤트의 카운트가 0에 가깝게 떨어지는 경우. 해당 PR이 그 에러 부류를 해결했음을 시사합니다.
- 노출 테이블의 행에 대상 코드의 피처 플래그 키가 적혀 있고 PR 배포 날짜 무렵 "shipped" / "concluded" 결정이 기록되어 있는 경우.

## 흔한 함정 (Common pitfalls)

- **계측됨 ≠ 원인 (Instrumented ≠ caused).** 이벤트가 존재한다는 것은 누군가 그것을 로깅할 만큼 신경 썼다는 뜻이지, 대상 코드가 그 이벤트 *때문에* 존재한다는 뜻은 아닙니다. 인과관계를 주장하기 전에 git 조사자가 찾은 PR/커밋 인용과 짝을 맞추세요.
- **조용한 계측 변경 (Silent instrumentation changes).** 이벤트 볼륨의 계단식 급증은 사용자 행동이 바뀐 것이 아니라 새로운 이벤트 로깅이 시작되었음을 의미할 수 있습니다. 볼륨 상승을 기능 출시 신호로 해석하기 전에 동일한 시간 창에 계측 PR이 있었는지 확인하세요.
- **스키마 드리프트 (Schema drift).** 이벤트 속성은 진화합니다. 오늘날 타입이 지정된 dbt 모델에 있는 컬럼이 대상 코드가 작성될 당시에는 존재하지 않았을 수 있습니다. 오래된 데이터는 원시 `properties_json` 내부에만 해당 속성을 가지고 있을 수 있습니다.
- **dbt 갱신 지연 (dbt refresh lag).** `<your_analytics_db>.<schema>.*`는 스케줄(보통 매시간/매일)에 따라 재구축됩니다. 최근 몇 시간 이내의 이벤트는 `your_warehouse.events.*`로 폴백하고 `_id`로 중복을 제거하세요.
- **회사별 고유 테이블 (Company-specific tables).** 실험, 피처 플래그, 과금, 사용량 테이블은 회사마다 다릅니다. 실제 존재 여부를 확인하지도 않은 테이블에서 결과를 보고하는 것은 전형적인 실패 모드입니다. 먼저 `SHOW TABLES` / `DESCRIBE TABLE`로 탐색하세요.
- **보존 기간 절벽 (Retention cliff).** 관련 시간 창이 테이블의 보존 기간이나 dbt 모델의 생성일보다 앞선다면, 이는 결과가 없는 것(null result)이 아니라 **공백(gap)**입니다. 통합자가 "결과 없음"을 "활동 없음"으로 오독하지 않도록 이를 명시적으로 기록하세요.
- **노트북은 쿼리 불가능 (Notebooks aren't queryable).** SQL MCP는 Databricks 노트북을 볼 수 없습니다. 근거가 노트북에 있다고 의심되면 공백(gap)으로 반환하세요.

## 반환할 내용 (What to return)

관련된 각 발견 항목에 대해:
- 유형 (제품 이벤트 / 실험 노출 / 사용량 또는 과금 이벤트 / 시스템 테이블 행 / dbt 모델)
- 전체 경로가 포함된(fully-qualified) 테이블 이름과 실행한 정확한 쿼리
- 쿼리한 시간 창(time window)
- 간결한 수치 요약 (카운트, 백분위수, 최초/최종 관측 타임스탬프). **원시 행(raw rows)을 그대로 덤프하지 마세요.**
- 대상 코드의 배포 날짜와의 시간적 상관관계 (예: "최초 행 2024-08-15, PR #49074 머지 2024-08-14")
- 관련성 + 강도: 직접적(direct) / 정황적(circumstantial) / 약함(weak)

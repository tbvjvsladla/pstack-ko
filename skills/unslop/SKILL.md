---
name: unslop
description: >-
  모든 글에서 AI 특유의 흔적(tells)을 덜어냅니다.
disable-model-invocation: true
---

# Unslop

텍스트를 편집하여 AI 패턴을 제거합니다. 아래 패턴들을 스캔한 뒤 다시 작성하세요. 원래의 의미는 보존하고 의도된 어조에 맞추세요.

## 감지 및 교정할 패턴

규칙 번호는 다른 스킬들이 인용하는 고유 ID입니다. 삭제된 규칙 번호는 빈자리로 남겨 둡니다.

### 내용 (Content)

3. **피상적인 -ing 구문.** "highlighting...", "ensuring...", "reflecting...", "showcasing...", "fostering..."(~을 강조하며, ~을 보장하며, ~을 반영하며 등). 삭제하거나 실제 출처와 근거를 들어 구체적으로 풀어쓰세요.
5. **모호한 출처 표기.** "전문가들은 믿는다(Experts believe)", "업계 보고서에 따르면(Industry reports suggest)", "일부 비평가들은 주장한다(Some critics argue)". 구체적인 출처를 밝히거나 삭제하세요.

### 언어 (Language)

7. **AI 상투 어휘.** Additionally(게다가), crucial(결정적인), delve(파고들다), enduring(지속적인), enhance(향상시키다), fostering(조성하는), garner(얻다), interplay(상호작용), intricate(복잡한), landscape(추상적 의미의 지형도), pivotal(중추적인), showcase(선보이다), tapestry(추상적 의미의 태피스트리/직조), testament(증거), underscore(강조하다), vibrant(활기찬). 평이한 일상 단어로 바꾸세요.
8. **"~이다(is)"를 화려하게 돌려 말하는 표현.** "serves as(~로서의 역할을 하다)", "stands as(~로 자리매김하다)", "boasts(~을 자랑하다)", "features(~을 특징으로 하다)". 그냥 "~이다(is)" 또는 "~을 가진다(has)"라고 쓰세요.
9. **"단순히 X일 뿐만 아니라 Y이다 (Not just X, but Y)."** 대신 요점을 직접 진술하세요.
10. **3의 법칙 (Rule of three).** 아이디어들을 억지로 세 개씩 묶지 마세요. 자연스러운 개수를 그대로 사용하세요.
11. **동의어 돌려막기 (Synonym cycling).** 한 문단 안에서 주인공을 두고 protagonist, main character, central figure, hero로 계속 바꿔 부르는 패턴. 하나를 골라 반복해서 쓰세요.
12. **가짜 범위 (False ranges).** X와 Y가 의미 있는 척도 위에 있지 않은데도 "X에서 Y에 이르기까지(from X to Y)"라고 쓰는 표현. 주제들을 직접 나열하세요.

### 스타일 (Style)

13. **줄표(Em dash, —) 남용.** 줄표는 아예 쓰지 마세요. 마침표나 쉼표만 사용하세요(괄호, 짧은 줄표(en dash), 대시 대용 하이픈도 쓰지 마세요). 생각을 분리해야 한다면 문장을 끝내거나 쉼표를 쓰세요.
14. **콜론(:) 남용.** 목록이나 예시 앞의 콜론은 괜찮지만 문장 중간을 잇는 연결자로 쓰지 마세요. "전통적인 자동화에서 넘어왔다면: 이벤트 핸들러를 등록하는 대신 조건을 기술합니다"에서 콜론은 아무 역할도 하지 못합니다. 비교 프레이밍 없이 요점이 스스로 서도록 다시 쓰세요: "스케줄러가 언제 작동해야 하는지는 평이한 문장으로 기술할 때 가장 잘 작동합니다." 지팡이 같은 문장 부호 없이도 동일한 의미를 전달합니다.
15. **굵은 글씨(Boldface) 남용.** 모든 고유명사나 약어마다 굵은 글씨를 쓰지 마세요.
16. **인라인 헤더 목록 (Inline-header lists).** 굵은 라벨과 콜론이 뒤에 오는 문장을 그대로 반복하는 것이 특징입니다: "**성능:** 성능이 향상되었습니다...". 이런 항목들은 일반 산문으로 바꾸세요. 마침표로 끝나며 항목 이름을 짓고 그 뒤에 실제로 새로운 세부 정보가 이어지는 굵은 도입부("**TypeScript로 작성된 스키마.** 테이블들이 하나의 파일에 모여 있습니다.")는 괜찮으며 AI 징후가 아닙니다.
17. **제목 스타일 대소문자 표기 (Title case headings).** 헤딩에는 문장형 대소문자 표기(Sentence case)를 사용하세요.
18. **장식용 이모지.** 헤딩과 불릿에서 이모지를 제거하세요.
19. **둥근 따옴표 (Curly quotes).** 곧은 따옴표(Straight quotes, `"` 및 `'`)를 사용하세요.

### 커뮤니케이션 군더더기 (Communication artifacts)

20. **챗봇 상투구.** "도움이 되었기를 바랍니다!(I hope this helps!)", "~가 필요하면 알려주세요(Let me know if...)", "물론이죠!(Of course!)", "그럼요!(Certainly!)", "결정적 단서를 찾았습니다!(Found the smoking gun!)" 모두 제거하세요.
22. **아첨하는 어조 (Sycophantic tone).** "좋은 질문입니다! 전적으로 맞습니다!" 본론으로 바로 답하세요.

### 군더더기 (Filler)

23. **군더더기 구절.** "In order to(~하기 위하여)"는 "To"로, "Due to the fact that(~라는 사실로 인하여)"는 "Because"로 바꿉니다. "It is important to note that(~라는 점에 주목하는 것이 중요합니다)"은 삭제하세요.
24. **과도한 유보 표현 (Excessive hedging).** "어쩌면 잠재적으로 ~일 수 있다고 주장될 여지가 있을지도 모른다(could potentially possibly be argued that it might)"는 "~일 수 있다(may)"로 바꿉니다.
25. **공허한 결론.** "미래는 밝아 보입니다(The future looks bright)." 구체적인 계획이나 사실을 진술하세요.

### 전문 용어 포장 (Jargon)

26. **추상적 은유 명사.** Substrate(기반/base), wedge, wedge in(추가하다/add), vector(방식/방법), locus, vantage, nexus, 명사로 쓰인 primitive, 은유로 쓰인 harness, "API surface"처럼 쓰인 surface, bedrock, 은유로 쓰인 scaffolding, modality, paradigm, gold-plating(작업에 필요한 것 이상의 과잉 작업), 은유로 쓰인 ratchet(메커니즘의 실제 이름이나 좁혀지기만 하는 제한으로 대체), 코드 이동을 뜻하는 evacuate(밖으로 옮기다/move out), endgame(마지막 단계), north star, flywheel. 이런 단어들은 기술적인 것처럼 읽히지만 대개 더 평이하고 구체적인 단어가 따로 있습니다. 구체적인 단어를 선택하세요.

### 평이한 화법 (Plain speech)

27. **어떻게 느껴지는지가 아니라 무엇을 하는지 말하세요.** "데이터베이스가 손닿는 곳에 머문다", "읽을 수 있는 SQL", "스키마를 따라가는 타입"은 느낌을 말할 뿐입니다. 교정된 문장은 메커니즘이나 숫자를 말합니다: "`.toSQL()`은 데이터베이스로 전송되는 정확한 문자열을 반환합니다", "열 이름을 바꾸면 빌드가 실패합니다". 그 문장이 독자에게 무엇을 하라고 하거나 무엇을 알게 해주는지 자문한 뒤 그것을 적으세요. 구체적인 지시, 사실 또는 숫자로 다시 진술할 수 없다면 덜어내세요. 한 가지 더: 그 문장을 다른 프로젝트의 문서에 그대로 옮겨 놓아도 어색하지 않다면 이 프로젝트에 대해 아무것도 말하지 않은 것입니다. 덜어내세요.
28. **빽빽한 문장은 줄이거나 쪼개세요.** 독자가 문장을 해석하려고 앞부분으로 되돌아가야 한다면 두 문장으로 나누거나 절을 덜어내세요. 한 문장에는 하나의 아이디어만 담습니다.
29. **능동태 (Active voice).** 능동태를 선호하세요. "is/are/was/were + 과거분사" 형태를 잡아내어 행위자를 명시하세요: "쿼리가 검증됩니다"는 "컴파일러가 쿼리를 검증합니다"로, "파일이 로더에 의해 파싱됩니다"는 "로더가 파일을 파싱합니다"로 바꿉니다. 수동태는 행위자를 알 수 없거나 정말로 중요하지 않을 때만 괜찮습니다.
30. **부사를 덜어내거나 더 강한 동사를 쓰세요.** "빠르게 실행됩니다(runs quickly)"는 "빠릅니다(is fast)" 또는 구체적인 수치로 바꾸고, "크게 개선합니다(significantly improves)"는 측정된 변화량(delta)으로 바꿉니다. 부사가 약한 동사를 떠받치고 있다는 것은 동사 선택이 잘못되었다는 뜻입니다.
31. **평이한 단어를 선호하세요.** "utilize"와 "leverage(활용하다)"는 "use(쓰다)"로, "facilitate(촉진하다)"는 "help(돕다)"로, "numerous(다수의)"는 "many(많은)"로, "in the event that(~한 경우에)"은 "if(만약 ~라면)"로 바꿉니다. 화려한 동의어가 더 명확한 경우는 드뭅니다.
32. **기교 부린 문체 (Mannered prose).** 직설적인 구절이 있는데도 쓴 은유나 수사적 기교: 경구 스타일("연결하든가 지우든가(wire it or delete it)"), 효과를 노린 수사적 단문 조각, 의인화된 코드("플랜이 그것을 붙들고 있다"), 비유적 동사("함께 올라탄다(rides along)", "~ 위에 서 있다(stands on)"), 상투적인 프레이밍 구절. "돌려볼 만한 다이얼(A dial worth turning)"은 "조절해 볼 만한 파라미터(a parameter worth varying)"로 바꿉니다. 뜻하는 바를 있는 그대로 말하세요. 은유 명사는 규칙 26이 다룹니다.
33. **과도한 압축 (Over-compression).** 독자가 글을 읽는 대신 암호를 해독하게 만드는 생략된 관사, 동사 없는 단편, 기호 나열식 표현, 약어. "Parser rejects bad date → exit 2, no write"는 "파서는 잘못된 날짜를 거부하고 코드 2로 종료하며 아무것도 기록하지 않습니다(The parser rejects a bad date, exits with code 2, and writes nothing.)"로 바꿉니다. 관사와 동사를 갖춘 온전한 문장으로 작성하고 화살표와 약어는 풀어서 적으세요.

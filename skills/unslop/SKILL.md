---
name: unslop
description: >-
  모든 글에서 AI 특유의 흔적(tells)을 덜어냅니다.
disable-model-invocation: true
---

# Unslop

텍스트를 편집하여 AI 패턴을 제거합니다. 아래 패턴들을 스캔한 뒤 다시 작성하세요. 원래의 의미는 보존하고 의도된 어조에 맞추세요.

규칙 번호는 다른 스킬들이 인용하는 고유 ID입니다. 삭제된 규칙은 빈자리로 남겨 둡니다.

3. 사실을 덧붙이지 않는 문미 "-ing" 구문("highlighting...", "ensuring...")을 쓰지 마세요.
5. 모호한 출처 표기("experts believe")를 쓰지 마세요. 출처를 명시하거나 삭제하세요.
7. AI 상투 어휘 금지: additionally, crucial, delve, enhance, foster, landscape, pivotal, showcase, testament, underscore, vibrant.
8. "serves as", "stands as", "boasts", "features" 대신 "is" 또는 "has"라고 쓰세요.
9. "not just X, but Y" 구문을 쓰지 마세요. 요점을 직접 진술하세요.
10. 아이디어들을 억지로 세 개씩 묶지 마세요.
11. 하나의 대상에는 하나의 이름만 쓰고, 그 이름을 반복하세요.
12. X와 Y가 실제 척도 위에 있지 않다면 "from X to Y"라고 쓰지 마세요.
13. 줄표(em dash), 짧은 줄표(en dash), 대시 대용 하이픈을 쓰지 말고, 그 자리에 괄호도 쓰지 마세요. 마침표나 쉼표를 사용하세요.
14. 콜론(:)은 목록이나 예시 앞에만 쓰며, 문장 중간의 연결자로 절대 쓰지 마세요.
15. 모든 고유명사나 약어마다 굵은 글씨를 쓰지 마세요.
16. 뒤에 오는 줄을 그대로 반복하는 굵은 라벨과 콜론("**Performance:** Performance improved...")을 쓰지 마세요. 마침표로 끝나며 새로운 세부 정보를 덧붙이는 굵은 도입부는 괜찮습니다.
17. 헤딩은 문장형 대소문자 표기(Sentence case)로 작성하세요.
18. 장식용 이모지를 쓰지 마세요.
19. 곧은 따옴표(Straight quotes)를 사용하세요.
20. 챗봇 상투구("I hope this helps!", "Let me know if...")를 쓰지 마세요.
22. 칭찬이나 동조로 여는 인사를 쓰지 마세요. 바로 본론으로 답하세요.
23. 군더더기를 덜어내세요. "In order to"는 "to"로 바꿉니다. "it is important to note that"은 삭제하세요.
24. 유보적 표현은 최대 한 종류만 사용하세요: "may".
25. 공허하고 일반적인 결론을 쓰지 마세요. 구체적인 계획이나 사실을 진술하세요.
26. 추상적 은유 명사 금지: substrate, wedge, vector, nexus, primitive, harness, surface, scaffolding, paradigm, ratchet, north star, flywheel. 구체적인 단어를 사용하세요.
27. 어떻게 느껴지는지가 아니라 무엇을 하는지 말하세요. 메커니즘이나 숫자를 명시하세요. 다른 프로젝트의 문서에 그대로 옮겨도 통할 만한 문장은 잘라내세요.
28. 문장당 하나의 아이디어만 담으세요. 독자가 다시 읽어야 하는 문장은 쪼개세요.
29. 능동태로 쓰세요. 행위자를 알 수 없거나 중요하지 않은 경우가 아니라면 행위자를 명시하세요.
30. 부사를 덜어내거나 더 강한 동사를 쓰세요. 숫자를 제시하세요.
31. 평이한 단어를 쓰세요: "utilize"나 "leverage" 대신 "use"를 쓰세요.
32. 문자 그대로의 직설적인 표현이 존재한다면 은유, 경구, 의인화된 코드, 비유적 동사를 쓰지 마세요.
33. 관사와 동사를 갖춘 온전한 문장으로 작성하세요. 화살표와 약어는 풀어서 적으세요.

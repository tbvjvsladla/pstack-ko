# 복사해 쓸 만한 프롬프트 레시피 (Prompts worth copying)

실제 경로, 스킬 이름, 완료 확인 기준으로 바꿔 넣으세요. 격식 없는 편한 문투로 적어도 잘 작동합니다.

## 이해하기 (Understand)

- `/poteto-mode read <thread>. restate the underlying issue in your own words, in plain english.`
  (`/poteto-mode <스레드>를 읽고 근본 문제를 네 자신의 말과 평이한 영어로 다시 서술해 줘.`)
- `/in-your-own-words <long or messy ask>`
  (`/in-your-own-words <길거나 어수선한 요청>`)
- `/sharpen <the prompt you're about to send>`
  (`/sharpen <보내려고 하는 프롬프트>`)
- `/poteto-mode investigate why <symptom>. give me what we know, what data you used, and your best hypotheses. don't change any code yet.`
  (`/poteto-mode 왜 <증상>이 발생하는지 조사해 줘. 우리가 아는 사실, 사용한 데이터, 가장 유력한 가설들을 알려줘. 아직 코드는 전혀 건드리지 마.`)
- `use /how to understand <subsystem>. then use /why to find out why it broke recently.`
  (`/how를 써서 <서브시스템>을 이해해 줘. 그런 다음 /why를 써서 최근에 왜 깨졌는지 알아내 줘.`)
- `/recall my work on <topic> from last week, then read <issue>.`
  (`/recall 지난주에 내가 한 <주제> 작업을 떠올린 다음 <이슈>를 읽어 줘.`)
- `/teach me why you implemented it this way and not <other way>. what did you trade off?`
  (`/teach 왜 <다른 방식>이 아니라 이 방식으로 구현했는지 나에게 가르쳐 줘. 어떤 트레이드오프를 감수했어?`)
- `/poteto-mode take over this branch. read the decision log, find what's done, and continue. don't redo finished work.`
  (`/poteto-mode 이 브랜치를 이어받아 줘. 의사결정 로그를 읽고 어디까지 완료되었는지 파악한 뒤 계속 진행해. 이미 끝난 작업을 다시 하지 마.`)

## 구축하기 (Build)

- 버그: `/poteto-mode <symptom>. repro first, then fix and verify.`
  (`/poteto-mode <증상>. 먼저 재현부터 한 뒤 수정하고 검증해.`)
- 앱 내부 버그: `/poteto-mode repro this with /verify-<app>. if it repros on main, fix it and show me a video as proof.`
  (`/poteto-mode /verify-<app>으로 이걸 재현해 봐. main에서 재현되면 수정하고 증명 비디오를 보여줘.`)
- 저렴한 테스트가 있는 버그: `/poteto-mode repro <bug> first. if there's a cheap test path, /tdd it. then fix and rerun.`
  (`/poteto-mode 먼저 <버그>를 재현해. 저렴한 테스트 경로가 있다면 /tdd로 진행한 뒤 수정하고 재실행해.`)
- 기능 추가: `/poteto-mode add <behavior>. <current output> stays byte-identical. verify both.`
  (`/poteto-mode <동작>을 추가해. <현재 출력>은 바이트 단위로 완전히 동일하게 유지되어야 해. 둘 다 검증해.`)
- 리팩터링: `/poteto-mode move <code> into one module, zero behavior change. record the current output first and prove it's unchanged after.`
  (`/poteto-mode 동작 변경 없이 <코드>를 하나의 모듈로 옮겨 줘. 먼저 현재 출력을 기록해 두고 작업 후에도 변함없음을 증명해.`)
- 성능 개선: `/poteto-mode <operation> takes <time> on <fixture>. trace it, fix the measured cause, show me before and after.`
  (`/poteto-mode <픽스처>에서 <작업>이 <시간>이나 걸려. 추적해서 측정된 원인을 고치고 전후 비교를 보여줘.`)

## 설계 및 계획 (Design and plan)

- `/poteto-mode prototype a few options for <feature>. take screenshots or videos for me to compare.`
  (`/poteto-mode <기능>에 대해 몇 가지 선택지를 프로토타입으로 만들어 줘. 내가 비교할 수 있게 스크린샷이나 비디오를 찍어 줘.`)
- `/poteto-mode we need <feature>. /architect it first, and answer open questions with prototypes. let me review before proceeding.`
  (`/poteto-mode <기능>이 필요해. 먼저 /architect로 설계하고 미해결 질문들은 프로토타입으로 답을 찾아 줘. 진행하기 전에 내가 리뷰할 수 있게 멈춰 줘.`)
- `/poteto-mode write a tutorial for how i would use <new package> first. then /teach me why it beats the current one.`
  (`/poteto-mode 내가 <새 패키지>를 어떻게 사용하게 될지 튜토리얼부터 먼저 작성해 줘. 그런 다음 왜 그게 현재 패키지보다 나은지 /teach로 설명해 줘.`)
- `ask /arena for a second opinion on this thread and our approach.`
  (`이 스레드와 우리의 접근 방식에 대해 /arena에게 세컨드 오피니언을 구해 봐.`)
- `/poteto-mode turn this design into a plan. small verifiable PRs, each with its own verification steps.`
  (`/poteto-mode 이 설계를 계획으로 변환해 줘. 각각 고유한 검증 단계를 갖춘 작고 검증 가능한 PR 단위로 쪼개 줘.`)
- `/poteto-mode plan the migration of <library> to <target>. small verifiable PRs. the result must match the original exactly, bugs included.`
  (`/poteto-mode <라이브러리>를 <타깃>으로 마이그레이션하는 계획을 세워 줘. 작고 검증 가능한 PR들로 나누고, 결과는 버그까지 포함해 원본과 정확히 일치해야 해.`)

## 리뷰 및 출하 (Review and ship)

- `/interrogate the whole branch, but skeptically. don't change anything yet. no nitpicks unless it's a real bug or regression.` (제외된 항목(Dropped) 목록도 함께 읽어보세요.)
  (`/interrogate 브랜치 전체를 회의적인 시각으로 심문해 줘. 아직 아무것도 고치지 마. 실제 버그나 퇴행이 아닌 자잘한 트집(nitpicks)은 빼고.`)
- `/create-formal-verification-skill prove <property> holds in <module>, even when <steps> interleave or retry.` (해당 코드에 머지나 리팩터링이 일어난 후에는: `/maintain-formal-verification-skill`.)
  (`/create-formal-verification-skill <단계들>이 서로 뒤섞이거나 재시도될 때조차 <모듈>에서 <속성>이 성립함을 증명해 줘.`)
- `/swarm check every package under <dir> against its check script. one worker per package. one report.`
  (`/swarm <디렉터리> 아래의 모든 패키지를 각자의 검사 스크립트로 점검해 줘. 패키지당 워커 하나씩 배정하고 단일 보고서로 정리해.`)
- `/poteto-mode open the pr. small ordered commits, evidence in the description.`
  (`/poteto-mode PR을 열어 줘. 순서가 잘 정리된 작은 커밋들로 구성하고 설명란에 근거를 넣어 줘.`)
- `/poteto-mode babysit this pr. get it green.` (상태만 확인할 때는: `/poteto-mode check on pr <number>. anything outstanding?`)
  (`/poteto-mode 이 PR을 돌봐 줘. 그린 상태로 만들어 줘.` / `/poteto-mode PR <번호> 상태를 확인해 줘. 미해결된 게 남아 있어?`)
- `/poteto-mode land the stack.`
  (`/poteto-mode 스택을 반영(land)해 줘.`)

## 자리 비움 및 복귀 (Away and back)

- `/poteto-mode im going to bed. <goal> in a fresh worktree off <base>. done means <checks>. keep a decision log. don't ask me before committing. /loop until done. if you're truly stuck after a few hours, stop and write up why.`
  (`/poteto-mode 나 자러 간다. <베이스>에서 분기한 새 워크트리에서 <목표>를 수행해. 완료 기준은 <검사들>이야. 의사결정 로그를 남기고 커밋 전에 물어보지 마. 끝날 때까지 /loop로 돌리고 몇 시간 후에도 정말 막혀 있으면 멈추고 이유를 적어 놔.`)
- `/show-me-your-work catch me up on what you did last night.` (Attention 섹션부터 먼저 읽으세요.)
  (`/show-me-your-work 어젯밤에 무슨 작업을 했는지 따라잡게 해 줘.`)
- `/ketchup`은 사용자의 마지막 메시지 이후 현재 채팅에서 일어난 일을 요약해 주고 사용자의 결정을 기다리는 모든 사안을 나열합니다.
- `/poteto-mode full autopilot on this queue. each item is independent.`
  (`/poteto-mode 이 큐를 완전 자율주행(autopilot)으로 돌려 줘. 각 항목은 서로 독립적이야.`)
- `/poteto-mode autopilot these changes, but don't merge. i'll land them.`
  (`/poteto-mode 이 변경사항들을 자율주행으로 처리하되 머지는 하지 마. 반영은 내가 직접 할게.`)
- `/reflect capture what we learned so the next run doesn't repeat it.` (미래의 의사결정을 실제로 바꾸는 편집안만 승인하세요.)
  (`/reflect 다음 실행에서 똑같은 시행착오를 반복하지 않도록 우리가 배운 점을 포착해 줘.`)
- `/bro`는 마지막 응답을 평이한 인간의 언어로 다시 설명해 줍니다.

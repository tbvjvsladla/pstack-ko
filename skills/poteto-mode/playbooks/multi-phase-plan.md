### 다단계 또는 다중 PR 계획 (Multi-phase or multi-PR plan)

**당신은 코드가 아니라 계획(plan)을 소유합니다. 계획은 소유자(owner)가 체크박스를 하나씩 실행하고 운영자(operator)가 증거로부터 감사(audit)하는 체크리스트입니다.** 계획 자체가 산출물입니다. 직접 구현하지 마세요.

1. 변경사항이 파일 한두 개 수준이고 접근법이 자명하다면 계획을 건너뛰세요. 그렇다고 말하고 멈추세요.
2. 계획을 작성하기 전에 미결 질문들을 프로토타입으로 먼저 해결하세요. 각 질문에 대해 `playbooks/prototype.md`를 실행하세요. Appendix A를 위해 브랜치, SHA, 스크린샷을 보관하세요. 운영자에게는 어떤 실행으로도 해결할 수 없는 제품 또는 선호도 결정에 대해서만 질문하세요. 질문할 때는 선택지를 제시하세요(**never-block-on-the-human** 원칙 스킬).
3. 서브에이전트(Subagents) 섹션에 따라 `subagent_type: "poteto-agent"`와 명시적 모델을 지정한 서브에이전트들에서 탐색을 수행하세요(**guard-the-context-window** 원칙 스킬). 서브에이전트 섹션에 따라 작업에 맞게 각 서브에이전트의 추론 수준과 빠른(fast) 또는 표준(standard) 속도를 선택하세요. 각 서브에이전트는 파일 포인터, 컨벤션, 테스트 명령어, 진입점을 반환합니다. 인라인 원문 덤프는 금지합니다.
4. 아래의 스켈레톤을 계획 파일에 복사하고 모든 플레이스홀더를 채우세요. 운영자가 경로를 지정하지 않는 한, 에이전트 스토어의 `docs/` 아래에 파일을 작성하세요. 모든 헤딩과 모든 하위 블록을 표시된 순서 그대로 유지하세요 (`check-plan.mjs`가 영문 헤딩과 규칙 문구를 검증하므로 코드 블록 내 영문 구조 키워드는 원문 그대로 유지합니다). PR당 하나의 섹션을 작성합니다. 하나의 PR은 자체적인 증거를 가진 하나의 변경입니다(**sequence-verifiable-units** 원칙 스킬). **How to read this**에 실행 플레이북을 명시하세요. 실행이 머지까지 수행할 수 있다면 `playbooks/autopilot-full.md`를 선택하세요. 운영자가 직접 PR들을 머지한다면 `playbooks/autopilot-stack.md`를 선택하세요. 상시 운영 프로그램은 `playbooks/orchestrate.md`를 사용합니다.
5. `/technical-writing`을 전체 적용하여 작성한 다음 `/unslop`을 적용하세요. 본문은 하나의 Diátaxis 모드(how-to)를 따릅니다. 부록(Appendices)에는 설명과 참조를 담습니다. 각 헤딩은 작업이나 발견 사항을 진술합니다. 긴 대시(long dashes)는 금지합니다. 문장 중간의 콜론(mid-sentence colons)도 금지합니다.
6. `node pstack/skills/poteto-mode/scripts/check-plan.mjs <plan.md>`를 실행하고 출력되는 모든 라인을 수정하세요(**encode-lessons-in-structure** 원칙 스킬).
7. 결과를 돌려주세요. 계획 파일 경로와 스크립트 출력을 게시한 뒤 멈추세요. 실행은 운영자의 명시적인 실행 승인(go)이 떨어졌을 때 계획에 명시된 실행 플레이북 아래에서 시작됩니다.

**검증 (Verification).** 테스트만으로는 충분한 검증이 되지 않습니다. PR은 단위(unit), 라이브(live), 성능(perf) 체크박스가 모두 체크되었을 때만 검증된 것으로 인정됩니다(`Tests alone are not sufficient verification. A PR is verified only when its unit, live, and perf boxes are all checked.` — **prove-it-works** 원칙 스킬). 이 문장이 검증 규칙입니다. 모든 검증 블록은 이 문장으로 시작합니다. 라이브 블록은 필수입니다. PR 헤드에서 10개의 레인이 **swarm** 스킬에 따라 `swarm workers` 모델(기본값 `grok-4.7-xhigh-fast`) 위에서 해당 표면의 제어 스킬을 통해 실제 표면을 구동합니다. 각 레인은 구체적인 시나리오, 저장하는 스크린샷, 통과 조건식(pass predicate)을 가진 하나의 체크박스입니다. 그중 한 레인은 **트렁크 대비 회귀 레인(`Regression lane against trunk.`)**입니다. 이 레인은 트렁크와 헤드에서 동일한 핵심 시나리오를 실행합니다. 트렁크에 해당 기능이 없다면 레인은 그 사실을 기록하고, 트렁크 결과를 지어내는 대신 diff가 추가하는 동작과 사용자가 기다리는 최종 상태를 게이트로 삼습니다. 성능 게이트는 양면(dual-sided) 구조입니다. 트렁크와 헤드 모두 명명된 지표를 산출해야 합니다. 트렁크에 해당 기능이 없다면 diff가 추가하는 작업을 분리하여 그 작업과 사용자가 기다리는 엔드투엔드 상태에 대한 절대 예산을 설정하세요. 서로 다른 시나리오 사이의 비율을 주장하지 마세요. 성능 블록은 지표, 교차 실행 프로브(interleaved probe), 먼저 측정된 트렁크 베이스라인, 그리고 실패 기준 수치가 포함된 규칙을 명시합니다. 인터랙션을 변경하는 PR은 리뷰 게이트 대상(review-gated)입니다. 머지 전에 운영자가 채팅에서 스크린샷과 비디오로 이를 리뷰합니다. 인터랙션을 변경하지 않는 PR은 `**Review gate.** None. <PR id> is not review-gated.`라고 적고 그 아래에 체크박스를 두지 않습니다.

**제어 스킬 (Control skill).** 표면에 따라 선택하세요. 브라우저, Electron, 웹 UI는 `cursor-team-kit`의 `control-ui`를 사용합니다. CLI와 TUI는 `cursor-team-kit`의 `control-cli`를 사용합니다. 네이티브 모바일은 저장소가 보유한 시뮬레이터 구동 스킬을 사용합니다. 두 표면을 건드리는 PR은 양쪽 모두에 레인을 둡니다. 제어 스킬이 없는 표면은 Appendix C에 리스크로 기록하며, 라이브 블록에는 여전히 각 레인이 해당 표면을 어떻게 구동하는지 명시합니다.

````markdown
# <Program> plan

<Under ten lines. What changes, for whom, the rule the program enforces, and the PR ids in order.>

## How to read this

One box is one unit of work. Every box names the evidence that checks it. A nested box is a sub-step of the box above it. Check a box only when its evidence exists, a file, a log line, a screenshot, a test run, or a SHA. The body is a how-to. The appendices explain and record.

The program runs `pstack/skills/poteto-mode/playbooks/<execution playbook>.md`. <Who merges, and which PR ids are the operator's items that stop at merge-ready.>

Tests alone are not sufficient verification. A PR is verified only when its unit, live, and perf boxes are all checked.

## Program checklist

### Arm the program

- [ ] State the protocol and this plan to the operator, then stop. Start execution only on the operator's explicit go.
- [ ] Read these from trunk at program start. Re-read them at every tick.
  - [ ] `git show origin/main:pstack/skills/poteto-mode/playbooks/<execution playbook>.md`
  - [ ] `git show origin/main:pstack/skills/swarm/SKILL.md`
  - [ ] `git show origin/main:<control skill path>`
  - [ ] `git show origin/main:pstack/skills/poteto-mode/playbooks/opening-a-pr.md`
  - [ ] `git show origin/main:pstack/skills/<each other leaf skill the program uses>`
- [ ] On the operator's go, arm the audit tick as `/loop 1h` with the tick prompt below. Never leave the cadence to memory.
- [ ] Use this tick prompt, verbatim. "Re-read the execution playbook from trunk. Audit the operation against it and fix drift in this tick. Probe every active lane and judge progress by side effects only. Stand down a stuck lane and dispatch its replacement now. Then post a short status message to the operator in chat only when the audit found a tracked change that no earlier status message reported, such as a PR opened, a code-ready head, a round launched or closed, a verdict, a merge, a stuck agent and the action taken, a blocker added or cleared, or a decision only the operator can make. Name every such change and nothing else. Do not repeat a table, the merged list, or an unchanged blocker. If the audit found none, end the turn with no reply text. Either way, log this tick's row in your decision trail. The row names the items reported, or none."
- [ ] On the operator's hold or stand-down, send every owner a zero-writes order at once.

### Spawn owners

- [ ] Spawn one owner per PR with the full lifecycle the execution playbook names.
- [ ] Follow this dependency graph. Start dependent work only after its parent merges, or base it on the parent branch when the execution playbook stacks.
  - [ ] <PR id> and <PR id> are independent and first. Both branch from `main`.
  - [ ] <PR id> after <PR id>.
- [ ] Hold the file boundaries. <PR id or class> touches only `<glob>`.
- [ ] Hold the review gate. <PR ids> change an interaction. They wait for the operator's review in chat with screenshots and a video before merge.

### PR mechanics, for every PR

- [ ] Resolve the forge once. Default to `gh`; if `command -v origin` succeeds and Origin can resolve the repository, use `origin pr` for every PR operation. Record any fallback to `gh`. Never require `gt`.
- [ ] Open the PR ready, never draft, per **Opening a PR**. Use the run's built-in PR tool when it has one, else `origin pr create --status open --base <base-branch>` or `gh pr create --base <base-branch>` according to the resolved forge. A stack child targets its parent branch.
- [ ] Run the repo's lint and typecheck once before the PR-facing push. Push with hooks on.
- [ ] Run `/deslop` before each commit and `/no-comments` before review.
- [ ] Triage every Bugbot and security-reviewer comment per `../references/bugbot-triage.md`.
- [ ] Rebase onto current trunk before the code-ready report and babysit. Keep that merge base in fix rounds. Rebase again only at merge prep, on a `git merge-tree` conflict with trunk, on a CI failure that comes from a change on trunk, or when a known concurrent change to the same files lands.

### Verdict and merge, for every PR

- [ ] A full swarm runs once, at the code-ready head. Another full swarm runs only for new behavior, a redesign, or a large new diff. A rebase, a merge-conflict resolution, or a minor fix gets a single verifier at the new head, and a wording-only change gets none. Run each full swarm per `pstack/skills/swarm/SKILL.md`. One gates lane. The ten live lanes from the PR's **Verify, live** block. The perf lane from its **Verify, perf** block. Two or more audit lanes, each with its own focus, that read the diff and the receipts and distrust the PR body. The root audits the receipts in the merge-ready report before the verdict.
- [ ] Judge every finding by the execution playbook's blocking rule. The verdict is clean when none blocks. A failure on trunk behavior in a lane marked `Regression lane against trunk.` is recorded against trunk and does not block the PR. A new head gets a fresh verdict, except for results that stay valid under the patch-id rule in `playbooks/shipping.md`.
- [ ] <The merge or handoff rule from the execution playbook, with the patch-id rule from `playbooks/shipping.md`.>

### Boot recipe, for every live lane

Each live lane runs on its own cloud VM at the PR head. Drive through `control-ui` or `control-cli` from `cursor-team-kit`.

- [ ] `git fetch origin <head-branch> && git checkout <head SHA>`.
- [ ] <Start the backend and the surface. Wait for ready.>
- [ ] <Deliver input only through the control skill's commands. Name the read-only diagnostics.>
- [ ] Save every screenshot to `/tmp/swarm-<pr-id>/worker-<n>/<slug>.png` and return the paths with the report.

## <Task as a verb phrase> (<PR id>)

**Depends on.** <PR id, or None.>

**Files.**

- [ ] Edit `<path>`.
- [ ] Create `<path>`.
- [ ] Delete `<path>`.

**Build.**

- [ ] <One change. Name the symbol and the file.>

**You see.**

- [ ] <One observable result, with the exact log line or screen state.>

**Verify, unit.** Tests alone are not sufficient verification. A PR is verified only when its unit, live, and perf boxes are all checked.

- [ ] <Test file and the case it gains.> Run `<command>`.

**Verify, live.** Tests alone are not sufficient verification. A PR is verified only when its unit, live, and perf boxes are all checked. Ten lanes on `<swarm workers model>` at the PR head, per the boot recipe.

- [ ] Lane 1. Regression lane against trunk. Run <the same load-bearing scenario> at trunk and head. If trunk lacks the feature, record that and gate <the behavior the diff adds plus the end state the user waits for>. Save `<slug>.png`. Pass when <predicate>.
- [ ] Lane 2. <Scenario.> Save `<slug>.png`. Pass when <predicate>.
- [ ] Lane 3. <Scenario.> Save `<slug>.png`. Pass when <predicate>.
- [ ] Lane 4. <Scenario.> Save `<slug>.png`. Pass when <predicate>.
- [ ] Lane 5. <Scenario.> Save `<slug>.png`. Pass when <predicate>.
- [ ] Lane 6. <Scenario.> Save `<slug>.png`. Pass when <predicate>.
- [ ] Lane 7. <Scenario.> Save `<slug>.png`. Pass when <predicate>.
- [ ] Lane 8. <Scenario.> Save `<slug>.png`. Pass when <predicate>.
- [ ] Lane 9. <Scenario.> Save `<slug>.png`. Pass when <predicate>.
- [ ] Lane 10. <Scenario.> Save `<slug>.png`. Pass when <predicate>.

**Verify, perf.** Tests alone are not sufficient verification. A PR is verified only when its unit, live, and perf boxes are all checked.

- [ ] Metric. <What is measured at both trunk and head. If trunk lacks the feature, also name the diff-added work and the end-to-end state the user waits for.>
- [ ] Probe. <The command or procedure, run at trunk and at the head, interleaved. Both sides must produce the metric.>
- [ ] Baseline. Record the trunk <value> first.
- [ ] Rule. <Head against trunk, with the number that fails. If the scenarios differ, add absolute budgets for the diff-added work and the user-visible end state instead of an invalid ratio.>

**Review gate.** The operator reviews before merge.

- [ ] Copy lane <n> screenshots into `<media path>/<pr-id>-review-<slug>.png`.
- [ ] Record a 30 to 60 second video of the change on a lane VM. Save it as `<media path>/<pr-id>-review.mp4`.
- [ ] Post the screenshots and the video in chat. Stop at merge-ready. Wait for the operator's click.

**Merge.**

- [ ] Root's clean verdict at the exact head SHA.
- [ ] Bugbot triage done.
- [ ] Rebased onto current trunk after the verdict, patch-id unchanged.
- [ ] <The owner squash-merges its own PR, or the root appends it to the base-branch stack and the operator lands it bottom-up.>

## Close the program

- [ ] Every box above is checked with its evidence.
- [ ] Reply to the operator with the report the execution playbook names.

## Appendix A. Prototype evidence

<Each open question a prototype answered, with the branch, the SHA, and the artifact links. Each question that stays unproven.>

## Appendix B. Alternatives rejected

<Each approach weighed and why it lost.>

## Appendix C. Risks

<Each risk with the PR it lands in and what the owner watches.>

## Appendix D. Links and reading list

<Docs to read before editing. Which PRs get `pstack/skills/how/SKILL.md` and `pstack/skills/interrogate/SKILL.md`. The trail per `pstack/skills/show-me-your-work/SKILL.md`.>
````

**응답(Reply):** 계획 파일 경로, 의존성 및 리뷰 게이트 대상 세트가 포함된 PR ID 목록, 프로토타입이 증명한 사항과 아직 증명되지 않은 사항, 그리고 체크 스크립트의 출력.

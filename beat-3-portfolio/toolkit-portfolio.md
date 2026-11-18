# Toolkit Portfolio

Path: `beat-3-portfolio/toolkit-portfolio.md`

**Student:** Thomas Lavadinho

## Featured Tools

I picked these three because each one changed for a real reason I can point to: a failed eval run, or a catch it made on my own work before it went public. repro-check and pr-precheck both changed after their eval harness told me something I didn't expect. plan-check changed less as a file, but it's the tool that most changed what I actually posted to the issue thread.

**repro-check**

**What it is, and the judgment hole I wrote:** It grades whether a reproduction report is ready to post on an issue. I wrote the rubric (five required checks: environment_recorded, steps_reproducible, behavior_matches_issue, honest_outcome, conventions_respected), the evidence guide, and my voice guide. The voice guide's rules are "no promises, only investigation", "show, don't assert", "name a deviation, don't hide it", and "credit what's already there".

**How it evolved, dated:** On 2026-09-28 my first runs had two false rejects (pkg-05, a conda setup, and pkg-09), so I loosened steps_reproducible and behavior_matches_issue. The next two full runs got worse: 18/20, then 17/20, and both failed the category floor. The loosening had flipped pkg-20, a disclosure case, into a false accept. The real gap was that conventions_respected never said what happens when a strict AI-disclosure policy meets a comment that says nothing about AI. I rewrote that check, confirmed it with a canary run on just pkg-20 and pkg-09, and only then spent a full run, which passed at 19/20.

**Evidence:** Before, conventions_respected only asked whether the comment followed the repo's conventions. After the rewrite, the check reads: "if the repo's policy requires disclosure of AI usage in any form (a 'strict' regime...), the comment must contain an explicit disclosure statement -- silence fails this check under such a policy; if the policy states no disclosure is required... or there is no AI policy at all, silence passes." Run history in `beat-1-sandbox/unit-2/reproduction.md`: 18/20 FAIL, 17/20 FAIL, then 19/20 PASS. The tool then gated my real repro comment on codepath/pathreview-ai301-fa26-s3 issue #61 (comment 5881075816).

**plan-check**

**What it is, and the judgment hole I wrote:** It grades whether a fix plan is ready to post before I write any code. I wrote six checks (diagnosis_grounded, scope_bounded, executable_by_stranger, test_plan_observable, honest_uncertainty, comms_thread_aware) and my first procedure.md, which sets the read order and what evidence to pull for each check. I also added two voice rules: "state an approach as a plan, not a certainty" and "defer to a maintainer's stated direction".

**How it evolved, dated:** The eval passed at 18/20 on its first full run (2026-09-28). I chose not to keep tuning it, because Unit 2 had just taught me that loosening one check can break another. The real change came in live mode the same day, on my plan for issue #61. The first live run rejected my plan. The second accepted it but still flagged a problem in my draft comment.

**Evidence:** First live run, REJECT: my plan had no test coverage for `api/routes/health.py`, even though CONTRIBUTING.md requires tests for every change. It also didn't address a mypy suppression in `pyproject.toml`. I added both to plan.md. Second live run, ACCEPT, with one flag: my draft comment said three classmates had asked a question when only two of them did, and it promised to check whether the bug pattern appeared elsewhere instead of just checking. Before posting, I fixed the credit to the two people who actually asked and answered the question directly with a grep result. That's the comment I posted (comment 5881531569).

**pr-precheck**

**What it is, and the judgment hole I wrote:** It grades whether a pull request is ready to open, reading the diff against the plan and the issue. This was the first unit where I wrote the SKILL.md frame myself, inside the staff CONTRACT. That covered the two modes, the scope and voice seams, the refusal rule, and the binary verdict. I also wrote four checks (plan_fidelity, test_evidence, diff_quality, standards_comms) and the procedure.

**How it evolved, dated:** On 2026-10-06 the first full run scored 15/20, and all five misses were the same thing: clear-accept packages rejected on test_evidence. My pass condition said the outcome of the repo's checks must be "shown, not just asserted," and the grader read any stated result, however specific, as an assertion. I rewrote the condition so that naming the check and its specific result counts. A canary run over the 5 false rejects plus one passing package from each other category scored 10/10, and the confirming full run scored 19/20.

**Evidence:** Before: "if the repo's own checks were run, their outcome is shown, not just asserted". After: "naming which check ran and its specific result (e.g. 'go test ./... passes') satisfies this -- pasted raw command output or logs are not required ... a generic unspecific assertion ('tests pass', 'all good') is what fails this". pkg-08 (a lazygit stash fix) went from reject to accept with no change to the package. The rubric also did real work on my own PR. Running the repo's actual checks against my branch (make typecheck) showed that removing the `call-overload` suppression, which my plan had assumed my fix resolved, was wrong. It was an unrelated Redis typing error. I restored it and disclosed it as a deviation in PR #99, which is exactly the "honestly disclosed deviation is not itself a fail" clause in plan_fidelity.

## Inventory

- issue-select: Unit 1 tool that ranks candidate issues against my fit profile (Python, API or data-parsing work, no RAG) with checks like maintainer_alive and nobody_on_it. It passed eval at 19/20 on 2026-09-16 and picked issue #61, but it barely changed after Unit 1.

## The Arc

**Beat 1**
Date: 2026-09-16
Friction: My sed command to set the repo line in issue-select's scope.md silently did nothing, because the real line started with "- Repo:" and not "Repo:", so the tool was still pointed at a placeholder.
Change: I replaced the placeholder with a small Python string replace and checked the file afterwards, and I reused that same pattern for every later tool's scope.md.
Result: Live mode ran against the real course repo, ranked issue #61 first and accepted it, and I claimed it.

**Beat 2**
Date: 2026-09-28
Friction: Loosening two repro-check checks to fix false rejects made the next two eval runs fail (18/20, then 17/20) because pkg-20 became a false accept.
Change: I rewrote conventions_respected to spell out the strict, no-requirement and no-policy cases, and I started using a canary --only run before spending a full run.
Result: The next full run passed at 19/20, and I used the canary habit in every unit after.

**Beat 3**
Date: 2026-09-28
Friction: plan-check rejected my live plan for #61 for missing test coverage, then flagged that my draft comment credited three classmates when only two had asked.
Change: I added a regression test and the pyproject.toml change to plan.md, fixed the credit, and answered the open question with an actual grep result.
Result: The plan comment I posted credited the right people and answered the question instead of promising to look later.

**Beat 4**
Date: 2026-10-06
Friction: pr-precheck's first full eval scored 15/20, with all five clear-accept packages rejected on test_evidence.
Change: I rewrote test_evidence so a named, specific check result counts as shown, and confirmed it with a 10-package canary run.
Result: The confirming full run scored 19/20 with every category floor intact.

**Beat 5**
Date: 2026-10-06
Friction: When I ran the repo's own checks before opening my PR, make typecheck failed on a Redis call-overload error that my plan had assumed my fix would make unnecessary.
Change: I restored the suppression, added aiosqlite as a dev dependency so my new test would run in CI, and wrote the reversal into the PR as a disclosed deviation.
Result: lint and typecheck passed and make test-unit showed 376 passed, and PR #99 opened against codepath/pathreview-ai301-fa26-s3 with the deviation explained.

## Limitations and What's Next

The biggest gap in my toolkit is that every tool reads text and can't run anything. plan-check accepted a plan built on a wrong assumption about the mypy suppression, because nothing in the plan's text looked wrong. Only running make typecheck caught it. My eval scores are also single runs on a nondeterministic grader, so 19/20 is one sample, not a guarantee. Next I'd give plan-check and pr-precheck a step that runs the repo's own checks, or at least requires their real output, before grading. I'd also run each eval three times and report the spread instead of one number.

**Tool limits.**

repro-check: it trusts what the report says about the environment and can't tell whether the steps would actually reproduce on a different machine.
plan-check: it can't verify claims that need code to run, so it passed my wrong call-overload assumption.
pr-precheck: I graded PR #99 by applying its rubric by hand instead of a full live-mode run, so its live path has not been exercised end to end on a real PR.

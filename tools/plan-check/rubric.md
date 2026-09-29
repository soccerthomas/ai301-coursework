# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis_grounded | the plan's stated cause, read against the repro evidence's observed artifacts (traceback, control run, expected-vs-actual) | the plan's stated cause is one the repro evidence's artifacts actually demonstrate, not merely a plausible-sounding guess; the plan targets the cause the evidence points at rather than just the symptom the issue describes; no claim goes beyond what the evidence shows | required |
| scope_bounded | the plan's in-scope / not-in-scope statement and its named files or areas | the change is one bounded, describable thing; an explicit not-in-scope line rules out adjacent changes, unrelated cleanup, or other issues in the same file; touching multiple files is fine only when all of them are necessary parts of that one change | required |
| executable_by_stranger | the plan's files/areas, approach, and order of work | a stranger could start working from the plan without asking the author anything first: specific files, functions, or lines are named and a concrete approach is stated, not "figure out the right fix" or other language that assumes the author's tacit knowledge | required |
| test_plan_observable | the plan's test plan section, read against the repro evidence's own steps and artifacts | the test plan names an observable outcome mapped onto the actual repro evidence already gathered (e.g. "re-run script X; step [1] should no longer raise and should match [2]'s result"), not a vague "verify it works" | required |
| honest_uncertainty | the plan's risks/unknowns section | genuine unknowns or risks are named specifically, not glossed over or dressed up as solved; the plan does not claim certainty about things it has not verified (whether the fix is complete, whether other call sites share the defect, etc.) | required |
| comms_thread_aware | the plan comment text, read against the issue thread's maintainer signals and the repo-facts block's stated conventions and AI-use policy | the comment accounts for anything a maintainer already suggested (follows it or explains a deviation), uses the repo's own terms rather than generic filler, and discloses AI assistance if the repo's policy requires it | required |

## Verdict rule

Accept (ready to post and build from) only if every required check passes. `unclear` on any required check counts as fail. There are no preferred checks. Verdict is accept or reject.

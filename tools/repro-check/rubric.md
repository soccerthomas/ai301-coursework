# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment_recorded | the repro report's Environment block | every dependency the reproduction path actually touches (language runtime, key libraries, DB/service versions, commit/branch) is named with a specific version, not "latest" or "recent"; any deviation from the repo's documented setup (skipped services, different OS, substituted DB) is stated explicitly rather than left implicit | required |
| steps_reproducible | the repro report's steps/commands block | steps start from a stated clean state (fresh clone, fresh venv/container) and are precise enough that a stranger could reconstruct the exact input without guessing -- either by showing the literal command/file content directly, or by naming the specific triggering element precisely enough (the exact field name, value, flag, or condition) that reconstructing an equivalent input introduces no ambiguity; vague or generic descriptions ("some malformed input", "a large file") do not qualify | required |
| behavior_matches_issue | the report's observed-output artifact (traceback, log excerpt, screenshot, or a stated non-occurrence) read against the issue's own description | the artifact shows the exact scenario the issue names was actually attempted, matching the issue's own trigger conditions, and shows what resulted -- either the defect reproducing with output matching the issue's stated symptom, or the defect not reproducing with the actual (clean) output shown as evidence -- not an unrelated error, and not a control run or non-reproduction whose outcome is left unstated | required |
| honest_outcome | the report's expected-vs-actual statement and any caveats | the stated result (reproduced / cannot-reproduce / partially reproduced) matches exactly what the artifacts show; no root cause claimed unless demonstrated, no "this probably also affects X" unless X was tested; an evidenced cannot-reproduce is a pass | required |
| conventions_respected | the claim/repro comment text against docs/CONTRIBUTING.md, the PR template, and any AI-use policy | if the repo's policy requires disclosure of AI usage in any form (a "strict" regime naming the tool and extent of assistance), the comment must contain an explicit disclosure statement -- silence fails this check under such a policy, since absence cannot be verified as non-use; if the policy states no disclosure is required for issue/claim comments, or there is no AI policy at all, silence passes; a claim comment promises investigation only, never a fix or a date; report uses the repo's own terms rather than generic template filler | required |

## Verdict rule

Accept (ready to post) only if every required check passes. `unclear` on any required check counts as fail. There are no preferred checks. Verdict is accept or reject.

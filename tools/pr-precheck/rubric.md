# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan_fidelity | the diff's changed files/hunks, read against the plan's in-scope/not-in-scope lines and any deviation notes; the description's claims about what changed, read against the diff | every changed file or hunk falls inside the plan's stated boundary or is covered by an explicit deviation note; the description does not claim more or less than the diff actually delivers; an honestly disclosed deviation is not itself a fail -- only an undisclosed mismatch between plan/description and diff is | required |
| test_evidence | the PR description's test-evidence section (before/after), read against the plan's test plan and the reproduction's own steps; the outcome of the repo's own checks (tests, type checker) if run | an observable behavior is named with a before state and an after state that map onto the plan's test plan, not a bare "tests pass"; if the repo's own checks were run, naming which check ran and its specific result (e.g. "go test ./... passes", "integration test stash_untracked_only_errors passes") satisfies this -- pasted raw command output or logs are not required, only a specific named outcome; a generic unspecific assertion with no named check or behavior ("tests pass", "all good") is what fails this | required |
| diff_quality | the unified diff and the commit list | the diff contains only the changes necessary for the one bounded fix (plus explicitly noted deviations); no debug leftovers, dead code, commented-out blocks, unrelated formatting churn, or drive-by edits to other functions | required |
| standards_comms | the PR description, read against the repo's PR template sections, CONTRIBUTING.md (including any AI-use disclosure policy), and any maintainer comments on the thread | every template section is filled with real content specific to this change, not boilerplate; AI assistance is disclosed if the repo's policy requires it; any maintainer-suggested direction is acknowledged or a deviation from it is explained | required |

## Verdict rule

Accept (ready to submit) only if every required check passes. `unclear` on any required check counts as fail -- an unverifiable claim fails the check it would have decided. There are no preferred checks. Verdict is accept or reject.

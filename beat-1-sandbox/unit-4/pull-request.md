# Unit 4: PR Precheck

GitHub username: soccerthomas

## Pull request

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/99

## pr-precheck verdict (live mode, graded against this PR)

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/pull/99",
  "checks": [
    {"name": "plan_fidelity", "grade": "pass",
     "evidence": "every changed file (health.py, test_health.py, pyproject.toml) falls inside the plan's scope; the one addition outside the original plan -- restoring the call-overload mypy suppression -- is disclosed explicitly in the description's Changes section as a deviation, with the reason (an unrelated pre-existing Redis() overload error) named."},
    {"name": "test_evidence", "grade": "pass",
     "evidence": "description names the observable (dependencies.postgres flips unhealthy->healthy), gives explicit before/after states, names the specific new test (test_postgres_probe_reports_healthy_when_reachable), and states make test-unit's actual result (376 passed, 53 pre-existing xfailed) rather than a bare 'tests pass'."},
    {"name": "diff_quality", "grade": "pass",
     "evidence": "diff touches exactly 3 files, each necessary for the bounded fix plus the one noted deviation; no debug leftovers, no unrelated edits to the redis probe or other routes."},
    {"name": "standards_comms", "grade": "pass",
     "evidence": "every PR template section (Summary, Issue, Changes, Testing, Screenshots, Notes for Reviewers) has real specific content; no AI-disclosure line needed since CONTRIBUTING.md states no AI-use policy for this repo; no maintainer-suggested direction on the thread to address."}
  ],
  "verdict": "accept"
}
```

## Run history

First full run (before the rubric fix): 15/20, below the 18/20 bar.
All 5 misses were false-rejects in the `clear-accept` category, every
one failing `test_evidence` with the same pattern: packages that named
a specific observable and a specific test/check result (e.g. "go test
./... passes") were being graded as if they'd only given a bare "tests
pass" assertion. The rubric's wording -- "their outcome is shown, not
just asserted" -- didn't say what counts as "shown," so the grading
model read any stated result, however specific, as an assertion.

Fix: rewrote the `test_evidence` pass condition to state explicitly
that naming the specific check and its result satisfies "shown" --
pasted raw logs aren't required, only specificity. Canary run across
all 5 false-rejects plus one already-agreeing package from every other
category: 10/10. Confirming full run: 19/20, PASS (one single-package
miss on a different, nondeterministic LLM-grading seed, not the
category this fix touched -- not worth chasing per the Unit 2 lesson
about regressions from over-fitting to one run).

## Package analysis

pkg-08 (clear-accept, jesseduffield/lazygit#5883 -- a stash-prompt
fix) is the clearest example of the bug. Its test evidence section
read:

> Integration test `stash_untracked_only_errors` passes; `go test
> ./...` passes; `go generate ./...` produces no cheatsheet diff.

Before the fix: reject, failed `test_evidence` -- the grading model
flagged this as an unverified "tests pass" claim, missing that it
names which test and which commands, which is exactly what the pass
condition was supposed to require. After the fix: accept, matching
gold. The check now explicitly distinguishes a named, specific result
from a generic one, so the same evidence reads correctly.

## Check rationale

`test_evidence`, as currently written:

> an observable behavior is named with a before state and an after
> state that map onto the plan's test plan, not a bare "tests pass";
> if the repo's own checks were run, naming which check ran and its
> specific result (e.g. "go test ./... passes", "integration test
> stash_untracked_only_errors passes") satisfies this -- pasted raw
> command output or logs are not required, only a specific named
> outcome; a generic unspecific assertion with no named check or
> behavior ("tests pass", "all good") is what fails this

This check is why PR #99 itself passes: the description states
`make lint`, `make typecheck`, and `make test-unit`'s actual pass
counts (376 passed, 53 pre-existing xfailed) rather than a blanket
"all checks pass," and names the specific new test by name.

## Trade-offs

The narrower `test_evidence` wording trades strictness for recall: a
PR that genuinely did run no tests and writes a vague "tests pass" to
hide it still fails (nothing named, no specific result), but a PR that
ran real checks and reported them in ordinary PR-description language,
without pasting console output, now passes instead of being
false-rejected. That's the right trade for this check's purpose --
the harness category it guards is `not-tested`, i.e. catching PRs with
no real verification, not PRs that verified but wrote about it
casually.

Separately: building this PR surfaced a real instance of what
`plan_fidelity` and `diff_quality` are meant to catch. My Unit 3 plan
assumed removing the `call-overload` mypy suppression was justified by
the SQLAlchemy fix. Running `make typecheck` against the actual diff
proved that assumption wrong -- the suppressed error was an unrelated,
pre-existing `Redis()` overload mismatch. I reverted it and disclosed
the reversal as a deviation in the PR description rather than quietly
dropping it, which is exactly the "honestly disclosed deviation is not
itself a fail" clause in `plan_fidelity` working as intended.

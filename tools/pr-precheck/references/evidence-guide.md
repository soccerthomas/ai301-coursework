# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

Where it lives: the plan's Scope section (in-scope / not-in-scope lines) and any deviation notes added during the build, read against the PR diff's changed files and the PR description's own claims about what changed.

What good looks like: every changed file or hunk maps onto the plan's stated boundary, or onto an explicit deviation note; the description's claims about what the PR does match the diff exactly -- neither broader (claiming a fix the diff doesn't deliver) nor narrower (silently doing more than described).

## Test evidence (harness category: not-tested)

Where it lives: the PR description's test/evidence section, read against the plan's test plan and the original reproduction's steps; the outcome of the repo's own checks (test suite, type checker, linter) if they were run.

What good looks like: names a specific observable (e.g. "dependencies.postgres flips from unhealthy to healthy"), shows a before state and an after state, and shows the repo's own checks' actual outcome rather than just asserting "tests pass."

## Diff quality (harness category: unreviewable)

Where it lives: the unified diff (or the PR's Files Changed view) and the commit history.

What good looks like: only the files and lines the plan named are touched; no leftover debug prints, no commented-out old code, no reformatting of untouched lines, no drive-by edits to unrelated functions.

## Standards and comms (harness category: standards-wall)

Where it lives: the PR description, read against the repo's `.github/PULL_REQUEST_TEMPLATE.md` sections and `docs/CONTRIBUTING.md` (including any AI-use disclosure policy), and any maintainer comments on the issue or PR thread.

What good looks like: every template section has real, specific content (not "N/A" or left blank); AI assistance is disclosed per the repo's policy; any maintainer-suggested direction is explicitly addressed rather than silently ignored.

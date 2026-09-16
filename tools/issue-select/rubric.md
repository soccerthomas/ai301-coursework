# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer_alive | last push date (repo-facts) + maintainer first-response sample | last push within 30 days of capture, OR at least one of the 5 sampled issues got a maintainer/owner/collaborator reply within 30 days of opening | required |
| repo_in_use | last push to any branch + latest release date | last push within 90 days of capture | required |
| scope_fits | issue body + comment thread | the issue asks for one clear objective, even if it touches multiple files or lists several related sub-items of the same kind; it is not explicitly framed as a tracking issue meant to be split into separate work items; no unresolved maintainer design debate; AND (opened or commented on by a maintainer/owner/collaborator, OR the objective and required change are already spelled out precisely enough to start from). Judge the size of the actual change being asked for, not the length of the writeup — boilerplate diagnostic dumps (CLI output, config, env info) do not count against scope. | required |
| nobody_on_it | assignees + linked PRs (repo-facts) + full comment thread | no assignee; no open linked PR; and no claim comment that is both recent (within ~180 days of capture) and unfollowed by further activity — an old claim with no subsequent activity, especially on a stale-marked issue, does not block | required |
| ai_policy_allowed | contribution policy line (repo-facts) | fail only on an outright ban on AI-generated/AI-assisted contributions; disclosure requirements, human-review requirements, or other conditions are not bans and pass; no policy stated also passes | required |

## Verdict rule

Accept only if every required check passes. `unclear` on any required check counts as fail. Verdict is accept or reject.

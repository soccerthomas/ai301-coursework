# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61

**Verdict output**

Live mode, one candidate, inside scope (codepath/pathreview-ai301-fa26-s3). Capture date: 2026-09-28.

Check Grade Evidence
maintainer_alive pass pushed_at 2026-09-16, 12 days before 2026-09-28 capture, within 30 (no maintainer reply in the 5-issue sample; #72 has 40 comments, all author_association NONE — passes only on the push-recency branch of the OR)
repo_in_use pass Same push, 12 days before capture, within 90; archived: false, no releases published.
scope_fits pass One objective — wrap the bare "SELECT 1" literal at api/routes/health.py:32 in sqlalchemy.text(). Opened by Aburke225 (COLLABORATOR), labeled good-first-issue/tier-1, no design debate in the thread.
nobody_on_it pass No assignee. One open linked PR (#82, fixes DB and Redis probes) and four classmate claim comments — all authors author_association: NONE, so the Path Review house rule and my PR clarification in scope.md exempt them.
ai_policy_allowed pass No AI_POLICY.md; docs/CONTRIBUTING.md has no AI restriction of any kind.

Every required check passes → accept. It also fits your profile well: pure Python, in api/ rather than rag/, and the whole change is a one-line import-and-wrap at a known line number.

Two things worth knowing before you start, neither of which changes the verdict:

docs/CONTRIBUTING.md says seeded bugs carry @pytest.mark.xfail(strict=True) tests you must delete as part of the fix, and api/routes/health.py's Redis probe has a separate attr-defined bug (#62) — not #61 — leave that one alone unless you take #62 too.
PR #82 already fixes #61 and #62 together, and four classmates have posted repro reports. You'll be duplicating work that exists, which the house rule says is fine for credit, but it means no merge likelihood for your own PR.

{
"item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61",
"checks": [
{"name": "maintainer_alive", "grade": "pass", "evidence": "repo pushed_at 2026-09-16, 12 days before 2026-09-28 capture, within 30 (no maintainer reply in the 5-issue sample)"},
{"name": "repo_in_use", "grade": "pass", "evidence": "last push 2026-09-16, 12 days before capture, within 90; archived: false"},
{"name": "scope_fits", "grade": "pass", "evidence": "bare "SELECT 1" at api/routes/health.py:32 needs sqlalchemy.text(); opened by COLLABORATOR Aburke225, labeled 'good first issue'/'tier-1', no design debate"},
{"name": "nobody_on_it", "grade": "pass", "evidence": "open linked PR #82 and all 4 claim comments are author_association NONE (classmates), exempt under the Path Review house rule and its PR clarification"},
{"name": "ai_policy_allowed", "grade": "pass", "evidence": "CONTRIBUTING.md contains no AI restriction; no AI_POLICY.md or AGENTS.md in the repo"}
],
"verdict": "accept"
}


---

## Eval iterations

**Run history**

19/20 agreement — a single run. My first full eval run already cleared the 18/20 bar with every category matched (`claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 3/4`), so no rubric revision or re-run was needed.

**Issue analysis**

issue-15 (zulip/zulip#19589 in the eval bundle): my rubric's verdict was `accept`; the gold label is `reject`. My rubric passed `nobody_on_it` because there is no assignee, no *open* linked PR (the two linked PRs, #20840 and #23123, are both closed), and the most recent claim comment (souvik150, 2024-02-16) is roughly two years older than the 2026-08-05 capture date — well past the check's ~180-day staleness window, so it reads as an abandoned, non-blocking claim. What the check misses is the pattern underneath: the thread shows three separate claim-then-unassign cycles (taeukkang09 blocked from claiming, ikrambil unassigned, SamChen41/SamchenUF unassigned) plus two closed-but-unmerged PRs, all on one issue. That is a "graveyard" signal — repeated good-faith attempts that never landed — which is different from "nobody has tried," and my rubric has no check that reads claim/PR history rather than current state, so it read the issue as open ground when the gold label treated the churn itself as disqualifying.

**Check rationale**

`nobody_on_it` as currently written: "no assignee; no open linked PR; and no claim comment that is both recent (within ~180 days of capture) and unfollowed by further activity — an old claim with no subsequent activity, especially on a stale-marked issue, does not block." It is written this way so the check screens for current contention only — a live assignee, an open PR, or a fresh unanswered claim — rather than penalizing an issue for any claim it has ever received. In an active repo almost every good first issue accumulates some abandoned history; blocking on all of it would reject nearly everything.

**Trade-offs**

What it gives up is exactly issue-15's failure mode: it cannot distinguish "nobody has touched this" from "several people have tried and failed," because both look identical once enough time has passed since the last claim. I am accepting that trade-off — a check that reads claim/PR history (counting past cycles, not just current state) would catch it, but it is a much heavier evidence read to ask a required check to carry, and would risk false-rejecting issues that simply took a few tries to staff correctly.

---

## Selection rationale

1. **Fit to interests and time available:** #61 is pure Python, in `api/`, and the fix is a single documented line (wrap `"SELECT 1"` in `sqlalchemy.text()`) — it matches my stated preference to avoid RAG/retrieval work and fits comfortably in the time I have this week.
2. **What the verdict identified correctly, and what I weighed that the rubric could not:** the rubric correctly confirmed the process checks (maintainer/repo activity, no AI-contribution ban, one clear objective). What it cannot weigh is personal fit or duplication risk: PR #82 already fixes #61 (and #62 alongside it), and four classmates have already posted reproduction reports. The Path Review house rule says a shared issue costs me nothing credit-wise, but I am choosing to write my own independent reproduction rather than lean on theirs.
3. **Anticipated difficulty in claiming:** low technically — the bug is small, well-documented, and already reproduced multiple times with working scripts I can adapt my own version of. The only friction is social: since #82 is already open, my PR is unlikely to be the one that merges upstream, though that does not affect course credit.

---

Related paths: `eval-run.txt` in this directory; my skill's files in `tools/issue-select/`.

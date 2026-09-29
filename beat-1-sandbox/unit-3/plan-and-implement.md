# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

---

## Posted upstream

**GitHub username**

soccerthomas

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61#issuecomment-5881531569

My plan: wrap the await db.execute("SELECT 1") call at
api/routes/health.py:32 in sqlalchemy.text(). My repro comment above
shows why this is the cause rather than a symptom — the same session runs
the identical SQL fine once wrapped in text() ([2]), so only the
raw-string form fails ([1], [3]).

Scope: this line, the import, a new test under tests/unit/test_health.py
(there isn't one covering this probe yet), and narrowing the
call-overload suppression in pyproject.toml for this module if the fix
clears it. Not touching the separate redis_health_check_failed path
below it — that's #62.

Bobaninja21 and rueiliu both asked whether this raw-string pattern shows
up at other execute() call sites in the repo. I checked: health.py:32
is the only bare-string execute() call in api/ or core/ — everything
else already uses select(...) or a pre-built stmt object, so this
looks isolated to this one line.

I'll verify by re-running my repro script against the patched code
(dependencies.postgres should flip to "healthy") and by adding a test
that asserts the same. I know PR #82 already exists for this issue; I'll
still open my own PR from my own branch per the house rules.


---

## Your branch

**Branch**

fix/61-health-select-text-wrap

**Evidence**

Before (from my Unit 2 repro comment, `PYTHONPATH=. python repro_61_thomas.py` against the pre-fix code):

[3] calling health_check() directly, as the endpoint does
2026-09-28 19:57:19 [error ] postgres_health_check_failed error="Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')"
2026-09-28 19:57:19 [error ] redis_health_check_failed error="'Settings' object has no attribute 'redis_host'"
2026-09-28 19:57:19 [debug ] vector_db_health_check_passed
-> 503 {'status': 'unhealthy', 'dependencies': {'postgres': 'unhealthy', 'redis': 'unhealthy', 'vector_db': 'healthy'}, 'safety_events_last_hour': 0, 'timestamp': '2026-09-28T23:57:19.276140'}


After (same script, same command, re-run against the built change on this branch):

[3] calling health_check() directly, as the endpoint does
2026-09-28 20:53:59 [debug ] postgres_health_check_passed
2026-09-28 20:53:59 [error ] redis_health_check_failed error="'Settings' object has no attribute 'redis_host'"
2026-09-28 20:53:59 [debug ] vector_db_health_check_passed
-> 503 {'status': 'unhealthy', 'dependencies': {'postgres': 'healthy', 'redis': 'unhealthy', 'vector_db': 'healthy'}, 'safety_events_last_hour': 0, 'timestamp': '2026-09-29T00:53:59.430825'}


`dependencies.postgres` flips from `"unhealthy"` to `"healthy"`, exactly as the plan's test plan predicted. The overall response is still `503` because `redis` is still `"unhealthy"` — that's #62, out of scope for this change, and expected rather than a regression. I also added `tests/unit/test_health.py`, which asserts this same postgres-healthy outcome directly and passes:

tests/unit/test_health.py::TestHealthCheck::test_postgres_probe_reports_healthy_when_reachable PASSED


And confirmed via `mypy` that the `call-overload` suppression for this module is no longer needed once the fix is applied (removed it from `pyproject.toml`; `python -m mypy api/routes/health.py` still reports "Success: no issues found").

---

## Eval iterations

**Run history**

18/20 — a single run. My first full eval run cleared the 18/20 bar with every category at or above floor (`clear-accept 5/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`), so no revision or re-run was needed.

**Package analysis**

`pkg-05` (conda/conda#16502 in the eval bundle): gold label is `accept`; my rubric's verdict is `reject`, failing on `honest_uncertainty`. The candidate plan states "Risk: none identified beyond one extra network request per channel per 24h, which is the constant's existing meaning." My check reads a bare "none identified" as a sign of false confidence — glossing over uncertainty rather than stating it. But in this package the confidence is actually earned: the thread shows a project MEMBER (danyeaw) already agreed to this exact approach (wiring `NOTICES_DECORATOR_DISPLAY_INTERVAL` into the cache check) before the plan was written, so a minimal-risk statement here is accurate, not glossed-over. My rubric's `honest_uncertainty` check has no way to distinguish a plan whose confidence is earned by maintainer sign-off from one that's simply avoiding hard questions, so it penalized both the same way.

**Check rationale**

`honest_uncertainty` as it currently reads: "genuine unknowns or risks are named specifically, not glossed over or dressed up as solved; the plan does not claim certainty about things it has not verified (whether the fix is complete, whether other call sites share the defect, etc.)." I wrote it this way to catch the failure family named in lecture — unknowns dressed up as certainty — since a plan that asserts everything is solved without evidence is exactly the kind of overconfidence that gets bad plans built and then reverted.

**Trade-offs**

What it gives up is exactly `pkg-05`'s failure mode: it can't tell earned confidence (a maintainer already signed off on the approach) from unearned confidence (the author just didn't look hard enough for risks), because both read identically as "no risks stated." I'm accepting that trade-off rather than trying to have the check cross-reference thread endorsement against the risk section — that cross-check is unreliable to automate consistently, and the cost is a rare false reject on an unusually well-supported plan, not a systemic miss on plans that actually hide real uncertainty.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; my skill's files in `tools/plan-check/`.

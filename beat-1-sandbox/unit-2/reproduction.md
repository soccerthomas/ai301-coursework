# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

---

## Your identity upstream

**GitHub username**

soccerthomas

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61#issuecomment-5880943222

Hi! I'd like to work on this as a first contribution. The issue points at
the health check's db.execute("SELECT 1") call in api/routes/health.py
— I haven't confirmed the mechanism myself yet, but I've seen a few
classmates already reproduce the resulting ArgumentError here. I'll set
up the repo from its own docs, reproduce it independently on my own
machine, and post my own repro report here before proposing anything.


**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61#issuecomment-5881075816

Reproduced on 2f4e82f52efbcfcc57d65b3fa5348672163ca088.

Environment: Python 3.13.15, SQLAlchemy 2.1.1, aiosqlite 0.22.1, FastAPI
0.141.1, structlog 26.1.0, WSL2 Ubuntu (kernel
6.18.33.2-microsoft-standard-WSL2), x86_64.

Deviation, stated up front: I don't have Docker available in my WSL2
distro, so I couldn't run the documented docker compose up -d + Postgres
stack. Instead I drove the real health_check() handler with an
AsyncSession bound to sqlite+aiosqlite. Run [2] below is the control
that shows why the substitution still answers the question: the error is
raised during SQLAlchemy's statement coercion, before any dialect-specific
behavior is involved, and the same session executes the identical SQL
successfully once it's wrapped in text(). I did not run this against a
live Postgres end to end; that confirmation is still worth having from
someone with the Docker stack up.

Steps: fresh clone of my fork at 2f4e82f, in a venv:

pip install "sqlalchemy>=2.0.0" aiosqlite greenlet fastapi structlog pydantic-settings "pydantic[email]" redis httpx

then, from the repo root, the exact script I ran (repro_61_thomas.py,
PYTHONPATH=. python repro_61_thomas.py):

import asyncio
import os

os.environ.setdefault("DATABASE_URL", "sqlite+aiosqlite:///./repro61_thomas.db")

import sqlalchemy
from sqlalchemy import text
from fastapi import HTTPException

from api.routes.health import health_check
from core.database import AsyncSessionLocal

async def main() -> None:
print(f"sqlalchemy version: {sqlalchemy.version}")
print(f"DATABASE_URL: {os.environ['DATABASE_URL']}")

async with AsyncSessionLocal() as session:
    print('\n[1] raw string call: await db.execute("SELECT 1")')
    try:
        await session.execute("SELECT 1")
        print("    -> no exception raised")
    except Exception as exc:
        print(f"    -> {type(exc).__name__}: {exc}")

async with AsyncSessionLocal() as session:
    print('\n[2] control: await db.execute(text("SELECT 1"))')
    result = await session.execute(text("SELECT 1"))
    print(f"    -> returned {result.scalar()!r}, no exception -- DB is reachable")

async with AsyncSessionLocal() as session:
    print("\n[3] calling health_check() directly, as the endpoint does")
    try:
        body = await health_check(db=session)
        print(f"    -> 200 {body}")
    except HTTPException as exc:
        print(f"    -> {exc.status_code} {exc.detail}")

asyncio.run(main())

Observed (verbatim):

sqlalchemy version: 2.1.1
DATABASE_URL: sqlite+aiosqlite:///./repro61_thomas.db

[1] raw string call: await db.execute("SELECT 1")
-> ArgumentError: Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')

[2] control: await db.execute(text("SELECT 1"))
2026-09-28 19:57:19,274 INFO sqlalchemy.engine.Engine BEGIN (implicit)
2026-09-28 19:57:19,274 INFO sqlalchemy.engine.Engine SELECT 1
2026-09-28 19:57:19,274 INFO sqlalchemy.engine.Engine [generated in 0.00032s] ()
-> returned 1, no exception -- DB is reachable
2026-09-28 19:57:19,275 INFO sqlalchemy.engine.Engine ROLLBACK

[3] calling health_check() directly, as the endpoint does
2026-09-28 19:57:19 [error ] postgres_health_check_failed error="Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')"
2026-09-28 19:57:19 [error ] redis_health_check_failed error="'Settings' object has no attribute 'redis_host'"
2026-09-28 19:57:19 [debug ] vector_db_health_check_passed
-> 503 {'status': 'unhealthy', 'dependencies': {'postgres': 'unhealthy', 'redis': 'unhealthy', 'vector_db': 'healthy'}, 'safety_events_last_hour': 0, 'timestamp': '2026-09-28T23:57:19.276140'}

Expected: the probe issues SELECT 1, the database answers, and
dependencies.postgres reads "healthy".

Actual: [1] the raw string call raises the exact ArgumentError this
issue names. [2] is the control: the same session runs the identical SQL
fine once wrapped in text(), returning 1 — so the database is reachable
and the statement itself is valid; only the raw-string form fails. [3]
shows the consequence at the route: the log line
postgres_health_check_failed fires, health_check()'s broad except Exception swallows the underlying error, and the endpoint answers 503
with postgres: "unhealthy" even though the database is up.

Not claiming: the redis_health_check_failed line in [3] is a separate
defect (AttributeError on settings.redis_host), tracked as #62 —
nothing here is evidence about that one.


---

## Eval iterations

**Run history**

18/20 (first full run; passed the 18/20 bar but with no margin) → 18/20 (after loosening `steps_reproducible` and `behavior_matches_issue` to fix two false rejects; agreement held but the `disclosure` category floor broke — `pkg-20` flipped to a false accept) → 17/20 (re-ran the same rubric; `disclosure` floor still unmet, plus a new false reject on `pkg-01`) → **19/20** (after tightening `conventions_respected` to handle silent AI-disclosure explicitly; `disclosure` category recovered to 1/1, category floor met — this is the run saved to `eval-run.txt`).

**Package analysis**

`pkg-05` (conda/conda#16543 in the eval bundle): gold label is `accept`; my rubric's final verdict is `reject`, failing on `steps_reproducible`. The candidate report names the trigger precisely (an `env.yml` with a valid `dependencies:` list plus an unrecognized `category:` section) without pasting the literal YAML file. I revised `steps_reproducible` to explicitly allow naming the specific triggering element instead of requiring literal file content, and a spot-check (`--only pkg-05`) passed under that wording — but the full run's grading is not perfectly deterministic, and this run happened to fail it again. I'm keeping the revised wording since it's the more correct rule; this one miss is grading variance around a borderline case, not a wrong rule.

**Check rationale**

`conventions_respected` as it currently reads: "if the repo's policy requires disclosure of AI usage in any form (a \"strict\" regime naming the tool and extent of assistance), the comment must contain an explicit disclosure statement -- silence fails this check under such a policy, since absence cannot be verified as non-use; if the policy states no disclosure is required for issue/claim comments, or there is no AI policy at all, silence passes." I added the explicit strict-vs-silent-vs-no-policy branching after `pkg-20` (a repo with a strict AI_POLICY.md requiring disclosure) twice slipped through as a false accept once I'd loosened two other checks — the original wording didn't say what to do when a strict policy meets a comment with no disclosure statement at all, so the grader was inconsistent about it.

**Trade-offs**

After that revision I re-ran `pkg-20` and `pkg-09` together as canaries via `--only` before spending the confirming full run: `pkg-20` (strict disclosure policy, no disclosure statement) correctly flipped back to `reject`, and `pkg-09` (a repo whose policy explicitly states no disclosure is required for issue comments) correctly stayed `accept` on silence. What the stricter rule gives up: it can't distinguish "silently used AI without disclosing" from "genuinely didn't use AI and just didn't say so" — both read as silence under a strict policy and both now fail. I'm accepting that trade-off, since a strict disclosure regime effectively asks for the statement regardless of actual use.

---

Related paths: `eval-run.txt` in this directory; my skill's files in `tools/repro-check/`.

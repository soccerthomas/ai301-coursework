# Plan: issue #61 — health check DB probe passes a raw SQL string

## Diagnosis

`api/routes/health.py:32` passes the bare string `"SELECT 1"` to `await
db.execute(...)`. Under SQLAlchemy 2.x, textual SQL must be wrapped in
`sqlalchemy.text()`; a bare string raises `ArgumentError: Textual SQL
expression 'SELECT 1' should be explicitly declared as text('SELECT 1')`.
That exception is caught by the surrounding `except Exception` (lines
35-38), logged as `postgres_health_check_failed`, and
`dependencies.postgres` is set to `"unhealthy"` even though the database
itself is reachable. Confirmed in my Unit 2 repro comment: run [2] shows
the identical session executing the same SQL successfully once wrapped in
`text()`, returning `1` with no exception.

## Scope

In scope: wrapping the `db.execute(...)` call at `api/routes/health.py:32`
in `sqlalchemy.text()`; adding the `text` import; adding a new test under
`tests/` covering this probe (none currently exists); narrowing the
`call-overload` type-check suppression in `pyproject.toml` for
`api.routes.health` if fixing the raw string clears that error.

Not in scope: the `redis_health_check_failed` line from the following
`try` block (`AttributeError` on `settings.redis_host`) — that's issue
#62. The `attr-defined` suppression in `pyproject.toml` belongs to #62 and
I'm leaving it alone.

## Files / areas

- `api/routes/health.py` — line 32 fix, plus import.
- `tests/` — a new test (no existing health test exists to modify),
  asserting `dependencies.postgres == "healthy"` when the DB is reachable.
- `pyproject.toml` — remove `call-overload` from the `api.routes.health`
  suppression list if the fix clears that type error; leave
  `attr-defined` in place (that's #62's).

## Approach

1. Add `from sqlalchemy import text` to `api/routes/health.py` if not
   already present.
2. Change line 32 to `await db.execute(text("SELECT 1"))`.
3. Add a test in `tests/` exercising `health_check()` with a reachable DB
   session, asserting `dependencies.postgres == "healthy"`.
4. Run the type checker against `api/routes/health.py`; if `call-overload`
   no longer fires, remove it from `pyproject.toml`'s suppression list for
   this module, keeping `attr-defined` (unrelated, #62).
5. Leave the redis `try` block and all other routes untouched.

## Test plan

Re-run `repro_61_thomas.py` from my Unit 2 repro comment against the
patched code (same SQLite substitution, still no Docker/Postgres).

Expected after: step [1] no longer raises `ArgumentError`, matching [2]'s
control (`returned 1, no exception`). Step [3] reports
`dependencies.postgres: "healthy"`. `dependencies.redis` may still read
`"unhealthy"` (#62, expected, not a regression). The new unit test passes,
and the type checker runs clean without the `call-overload` suppression
for this module.

## Risks / unknowns

- I haven't checked whether the same raw-string pattern appears at any
  other `execute()` call site in this repo; two classmates raised this
  same question in the thread and I haven't resolved it either.
- I haven't run this against a live Postgres end to end, only SQLite.
- I don't yet know for certain that removing the `call-overload`
  suppression will actually pass cleanly until I run the type checker
  against the fixed code — if it doesn't, I'll leave the suppression and
  note why in the PR.
- PR #82 already exists addressing #61 and #62 together, so my own PR is
  unlikely to be the one that merges upstream.

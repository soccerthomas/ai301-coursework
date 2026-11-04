# Contribution Story

Student: Thomas Lavadinho

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61

Pull Request: https://github.com/codepath/pathreview-ai301-fa26-s3/pull/99

## Toolkit

I chose issue-scout, pr-precheck, voice-guard, ship-gate, and pr-monitor because these were the tools that I used the most throughout the course. They also show how my toolkit changed over time. At first I was mainly trying to find good issues to work on, but later I started using tools to check my work and make sure a pull request was actually ready before opening it.

### issue-scout

What it is, and the judgment hole I wrote: issue-scout helps me look through an issue and decide if it is something I should work on. The part I had to decide myself was what makes an issue a good choice. I looked at things like how clear the issue was, how big it seemed, and whether I had enough information to actually work on it.

How it evolved, dated: In October 2026, I started using issue-scout mainly to help organize information about possible issues. As I used it more, I realized the useful part was not having the tool choose an issue for me. It was having the information organized enough that I could make the final decision myself.

Evidence: issue-scout stayed as part of my toolkit even after I started adding tools for checking pull requests. I still needed it at the beginning of the process to help decide what work was realistic before moving on to planning and coding.

### pr-precheck

What it is, and the judgment hole I wrote: pr-precheck checks my work before I open a pull request. My judgment is in the rubric that decides what should be checked and what counts as ready. It can look at my code changes along with my plan, testing evidence, and PR draft.

How it evolved, dated: In October 2026, I first built pr-precheck during Unit 4. At first it was being used in the course environment, but later I had to use it on a real repository and branch. By Unit 7, ship-gate was using my pr-precheck as part of its own check, so my original tool became part of a bigger workflow.

Evidence: During Unit 7, ship-gate runs pr-precheck on the actual branch diff along with plan.md, test_evidence.md, and the PR draft. If something does not meet my rubric, the tool can report what did not pass so I know what to look at next.

### voice-guard

What it is, and the judgment hole I wrote: voice-guard checks the writing in my pull request. I created a voice guide with rules for how I wanted the PR to sound. The main goal was to avoid sending unclear or bad PR descriptions even if the code itself was okay.

How it evolved, dated: In October 2026, I worked on the voice guide during Unit 6. Later it was moved into its main location so other tools could use it. By Unit 7, voice-guard was connected to the PR workflow instead of just being something I checked separately.

Evidence: One of the Unit 7 tests was running:

`claude "run this command: gh pr create --help"`

Since there was no PR body, voice-guard was supposed to stop the command. This helped show that it was actually connected to the PR process.

### ship-gate

What it is, and the judgment hole I wrote: ship-gate is the final check before opening a pull request. It runs the checks that I put in ship-manifest.md and then runs my pr-precheck. The part I had to decide was which tests or checks were important enough that they needed to pass before I asked somebody else to review the PR.

How it evolved, dated: In October 2026, during Unit 7, I installed ship-gate and created the manifest for my repository. This was different from the earlier tools because it connected several parts of my toolkit together. Instead of checking everything separately, I could run one gate before opening the PR.

Evidence: I could run the gate with:

`python3 ~/.claude/ship-gate/check_gate.py --branch --title "<PR title>" --body-file pr_draft.md`

The gate runs the checks in my manifest and then my pr-precheck. Depending on the branch, it can pass, block the PR and explain why, or refuse if something needed for the check is not ready.

### pr-monitor

What it is: A runner that reads my PR's live state with read-only gh calls and drafts the matching next move to a file, using my own playbook.

How it evolved, dated: In November 2026, during Unit 8, I installed the PR monitor and wrote my playbook, with a 21-day first-response threshold because no PR in this repo had been merged yet.

Evidence: On Nov 3, 2026 it drafted my first and only nudge on PR #99. I checked it against my voice guide and posted it ([comment](https://github.com/codepath/pathreview-ai301-fa26-s3/pull/99#issuecomment-6619856139)). My walk-away rule now gives it 30 days.

### unit-1 harness

What it is: A tool from the beginning of the course that I used for testing and evaluation. It stayed part of my toolkit while I added the newer tools.

**Beat 1**

Date: October 2026

Friction: I needed a better way to decide which issues were actually realistic for me to work on instead of just picking one that looked interesting.

Change: I used issue-scout to organize the important information about each issue and added my own criteria for deciding if an issue was a good choice.

Result: I had a more organized way to decide what issue I wanted to work on.

**Beat 2**

Date: October 2026

Friction: After working on an issue, I did not have one clear way to decide whether the contribution was actually ready for a pull request.

Change: I created pr-precheck and used my own rubric to check the code changes, plan, testing evidence, and PR draft.

Result: I had a checklist that could point out things I still needed to fix before opening the PR.

**Beat 3**

Date: October 2026

Friction: Even if the code was ready, the PR description could still be unclear or not follow the writing style I wanted.

Change: I created a voice guide and used voice-guard to check the PR text.

Result: The writing became another part of the PR process instead of something I only checked at the very end.

**Beat 4**

Date: October 2026

Friction: I had several tools that worked separately, but I still had to remember to run everything before opening a pull request.

Change: I installed ship-gate and connected the repository checks, my pr-precheck, and the PR-opening process together.

Result: I had one final place where my work could be checked before I tried to open the pull request.

**Beat 5**

Date: November 3, 2026

Friction: After opening PR #99, I had no clear rule for when silence meant I should wait and when it meant I should follow up.

Change: I installed pr-monitor and wrote a playbook with my own staleness threshold, nudge etiquette, and walk-away criteria.

Result: The monitor drafted one nudge because my threshold was met. I checked it against my voice guide and posted it, and my walk-away rule now decides what happens next.

The tools are useful, but they still depend a lot on the rules and information I give them. If I write a bad rubric or leave an important check out of the manifest, the tool might not catch something that matters. I also think the tools could be easier to use together. Right now there are still different files and reports that I have to keep track of.

If I kept working on the toolkit, I would probably make the reports simpler and make it easier to see the whole process in one place. I would also keep adjusting the rules based on problems I actually run into instead of trying to predict every possible problem ahead of time.

Tool limits.

issue-scout: It only knows the information it can find about the issue and repository, so it can still miss context that a maintainer knows.

pr-precheck: It depends on the rubric I wrote, so it can miss something if my rubric does not cover it well.

voice-guard: It checks against my voice guide, but good writing can still depend on the specific repository and maintainer.

ship-gate: It depends on the manifest being set up correctly, and some checks might not be possible to run on my computer.

## Why I Chose This Issue

I picked issue #61 because it seemed like a manageable issue to start with, and the description was clear enough that I could understand what needed to be done. It was also a good opportunity to work with Python and SQLAlchemy, which I already had some familiarity with. I wanted something that would let me contribute without spending too much time just figuring out the overall codebase.


## Understanding the Issue

The `/health` route's postgres probe in `api/routes/health.py` called `db.execute("SELECT 1")` with a raw SQL string. SQLAlchemy 2.x only accepts executable constructs, so the call raises `ArgumentError: Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')` during statement coercion, before it ever reaches the database.

`health_check()` wraps each probe in a broad `except Exception`, so the error is swallowed and the endpoint answers `503` with `postgres: "unhealthy"` even when the database is up. The redis probe failing in the same response is a separate defect (#62) and not part of this issue.


## Reproduction Process

Before proposing anything I claimed the issue and said I would reproduce it myself, since classmates had already posted reproductions and I wanted my own evidence rather than repeating theirs.

I reproduced on commit `2f4e82f` with Python 3.13.15, SQLAlchemy 2.1.1 and aiosqlite 0.22.1 on WSL2 Ubuntu, using my own script (`repro_61_thomas.py`) that drives the real `health_check()` handler.

**Deviation:** I don't have Docker in WSL2, so I couldn't run the documented Postgres stack. I used an `AsyncSession` bound to `sqlite+aiosqlite` instead. The error is raised during SQLAlchemy's statement coercion, before any dialect-specific behavior, so the substitution still answers the question. I did not confirm it against a live Postgres end to end.

The script ran three steps:

1. Raw string `db.execute("SELECT 1")` raised the exact `ArgumentError` the issue names.
2. Control: the same session ran `db.execute(text("SELECT 1"))` and returned `1`, so the database was reachable and the SQL was valid. Only the raw-string form failed.
3. Calling `health_check()` directly logged `postgres_health_check_failed` and returned `503` with `postgres: "unhealthy"`.


## Solution Approach

I posted my plan on the issue thread first: wrap the probe's `SELECT 1` in `sqlalchemy.text()`, the form SQLAlchemy 2.x itself asks for in the error message. It is the smallest change that fixes the cause instead of the symptom.

I kept the PR to #61 only. The redis probe failure (#62) showed up in the same response, but it is a separate, pre-existing bug, so I left it untouched and said so in the PR.


## Testing Strategy

- **Regression test:** I added `tests/unit/test_health.py` with `test_postgres_probe_reports_healthy_when_reachable`, which uses an in-memory SQLite substitute and asserts the postgres probe reports `healthy`.
- **Repro re-run on the branch:** before the fix the raw-string call raised `ArgumentError` and postgres was always `unhealthy`; after it, `text("SELECT 1")` succeeds and postgres reports `healthy`.
- **Full suite:** `make test-unit` passed (376 passed, 53 pre-existing xfailed, unrelated to this change). `make lint` and `make typecheck` both passed.
- **CI:** all five jobs are green on the PR.
- **Not applicable:** integration tests (no integration-level touchpoints) and removing an xfail marker (I checked `tests/` and no xfail test covered this bug).


## Implementation Notes

- `api/routes/health.py`: wrapped the postgres probe's `SELECT 1` in `text()` and added the `sqlalchemy.text` import.
- `tests/unit/test_health.py` (new): the regression test above.
- `pyproject.toml`: added `aiosqlite` as a dev dependency for the test's SQLite substitute, and restored the `call-overload` mypy suppression for `api.routes.health`.

**Deviation from my plan:** I expected this fix to also remove the need for the `call-overload` suppression. `make typecheck` showed that suppression covers an unrelated, pre-existing `Redis()` overload mismatch, so I restored it rather than leave typecheck red, and noted it for reviewers in the PR.


## Pull Request

PR: https://github.com/codepath/pathreview-ai301-fa26-s3/pull/99

Status: Open as of Nov 3, 2026. 5 checks passing, mergeable, no review yet.

Maintainer Feedback: None yet as of Nov 3, 2026. I posted one nudge on Nov 3, 2026 ([comment](https://github.com/codepath/pathreview-ai301-fa26-s3/pull/99#issuecomment-6619856139)).


## Learnings and Reflections

Working on a real open-source repository is pretty different from working on personal projects or assignments. Even when an issue seems straightforward, you have to understand how the existing code is structured and make sure your changes fit the project's conventions. My one-line fix still needed a regression test, a new dev dependency, and a mypy suppression I expected to remove but had to keep, because `make typecheck` showed it covered an unrelated `Redis()` problem.

I was also surprised by how much time goes into understanding the code before writing anything. Next time, I'd spend more time exploring the repository, reading related code, and checking existing tests before jumping into implementation.


## Resources Used

- Issue #61: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61
- Issue #62 (redis probe, kept out of scope): https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62
- SQLAlchemy `text()` documentation: https://docs.sqlalchemy.org/en/20/core/sqlelement.html#sqlalchemy.sql.expression.text
- SQLAlchemy 2.0 migration guide: https://docs.sqlalchemy.org/en/20/changelog/migration_20.html
- The repo's README and Makefile targets (`make test-unit`, `make lint`, `make typecheck`)
- My course toolkit (issue-scout, pr-precheck, voice-guard, ship-gate, pr-monitor) and Claude Code

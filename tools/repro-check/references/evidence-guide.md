# Evidence guide: where proof lives in a reproduction package

## Environment

Where it lives: the repro report's "Environment" section — OS, language/runtime version, key dependency versions, and the repo commit/branch. In live mode, checked against the repo's own documented requirements (docs/SETUP.md, pyproject.toml/requirements pins).

What good looks like: every dependency the reproduction path actually touches is named with a specific version. If the student's setup differs from the repo's documented one (skipped Docker services, substituted SQLite for Postgres, different OS), that deviation is called out up front, with a reason it shouldn't change the result.

## Steps

Where it lives: the report's numbered steps or command block.

What good looks like: starts from a stated clean starting point (fresh clone, fresh venv), each command is copy-pasteable as written, and the exact input that triggers the bug is shown directly rather than described abstractly ("some malformed input" is not a step; the literal string or file is).

## Behavior shown

Where it lives: the report's observed-output block — raw traceback, log lines, or a screenshot — plus a control run when one is included.

What good looks like: the artifact quotes the exact error or output the issue names (matching error type, message text, or output shape). A control run (the same operation with valid input, or the working case) is shown separately, so the report demonstrates isolation, not just "something failed."

## Honesty

Where it lives: the report's Expected vs. Actual statement, and any stated caveats about what wasn't tested.

What good looks like: the claim matches the evidence exactly. No root cause asserted unless the artifact demonstrates it. No claim that a fix will also cover a related case unless that case was actually run. A failed reproduction attempt is reported as a failed attempt, not reframed as a success or silently dropped.

## Comms

Where it lives: the claim comment and repro comment text, checked against the repo's docs/CONTRIBUTING.md, .github/PULL_REQUEST_TEMPLATE.md, and any AI-use policy section.

What good looks like: discloses AI assistance where the repo's policy asks for it, echoes the repo's own terminology and labels rather than generic boilerplate, and — for a claim comment specifically — promises only investigation, never a fix or a date.

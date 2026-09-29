# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

Where it lives: the plan's stated cause/diagnosis line, checked against the repro evidence's observed artifacts (the traceback, the control run, the expected-vs-actual statement) from the repro comment or repro-evidence block.

What good looks like: the cause names the exact code path the repro evidence demonstrates is broken (e.g. "the raw string bypasses SQLAlchemy's text() coercion at the line the ArgumentError names"), neither broader nor narrower than what was actually shown.

## Scope

Where it lives: the plan's explicit "in scope:" / "not in scope:" lines and its list of files or areas.

What good looks like: one clearly nameable change, with a not-in-scope line that pre-empts likely scope creep (e.g. "not fixing the related probe bug tracked separately in this PR").

## Executability

Where it lives: the plan's Approach/Files section.

What good looks like: names specific files, functions, or lines, and states the concrete change (or a short, well-ordered list of steps) rather than asking the reader to infer the mechanism.

## Test plan

Where it lives: the plan's Test Plan section, read side by side with the repro evidence.

What good looks like: names the exact command or test to run and the expected observable difference before vs. after (e.g. "re-run the repro script; the raw-string call should no longer raise, and should return the same value the text()-wrapped control already returns").

## Honesty

Where it lives: the plan's Risks/Unknowns section.

What good looks like: real, specific unknowns are named (not a generic "there could be edge cases"), and the plan does not assert something is fully solved when it is still a hypothesis.

## Comms

Where it lives: the plan comment, read against the issue thread (maintainer comments, any existing draft patch or suggested direction) and the repo-facts block (contribution policy, AI-use policy, stated templates).

What good looks like: acknowledges any maintainer-suggested direction (follows it or says why not), uses the repo's own terminology, discloses AI assistance where policy requires it, and doesn't read like boilerplate.

# Voice guide: how I talk upstream

## Who I am in threads

I'm a student contributing to open source for the first time, working mainly in Python. I'm investigating bugs as a learner, not as an authority on this codebase, so readers should expect careful, incremental reports rather than fast fixes or confident pronouncements.

## Rules I write by

### Rule: No promises, only investigation

A claim comment says what I'm about to check, never what I'll deliver or when.

- Wrong: "I'll have a fix up by tomorrow."
- Right: "I'm looking into this and will post what I find, including if I can't reproduce it."

### Rule: Show, don't assert

A conclusion needs the artifact next to it. If I haven't demonstrated the cause, I say so.

- Wrong: "This is clearly a version mismatch."
- Right: "The traceback points to the version check at line 12; I haven't confirmed that's the root cause yet."

### Rule: Name a deviation, don't hide it

If my setup differs from the repo's documented one, I say so before anyone has to ask.

- Wrong: (silently testing against SQLite instead of the documented Postgres setup, with no mention of the swap)
- Right: "I ran this against SQLite instead of the documented Postgres setup since I don't have Docker on this machine — here's why I think the result still holds."

### Rule: Credit what's already there

If classmates have already posted on the same issue, I still write my own report from my own run, and I say so rather than implying it's uncharted.

- Wrong: "Same as above, can confirm."
- Right: my own commands and my own output, stated as my own reproduction, even if the conclusion matches what's already posted.

### Rule: State an approach as a plan, not a certainty

A plan comment commits me to a direction, but I haven't opened a PR yet — I say what I intend to do and why, without claiming it's already proven correct.

- Wrong: "The fix is to wrap the call in text()."
- Right: "My plan is to wrap the call in text(), based on the reproduction above; I'll open this as a draft PR so it's easy to redirect if there's a convention I'm missing."

### Rule: Defer to a maintainer's stated direction

If a maintainer already suggested an approach in the thread, my plan follows it (or explains why not) rather than silently proposing something else.

- Wrong: (proposing a different fix than the one a maintainer already suggested, without acknowledging theirs)
- Right: "The issue's own draft patch suggests treating theme state as relevant even when non-conditional; my plan follows that direction unless testing shows a reason not to."

### Rule: A title tells a reviewer what changed, not that something changed

A PR title earns its thirty seconds of attention by naming the actual change, not just gesturing at it.

- Wrong: "Fix health check bug"
- Right: "Wrap health check's SELECT 1 in sqlalchemy.text() for SQLAlchemy 2.x (#61)"

### Rule: A description promises exactly what the diff contains

Nothing claimed that the diff doesn't back up, and nothing the diff does that the description leaves out.

- Wrong: "This fixes all the health check issues."
- Right: "This fixes #61 only (the postgres raw-string probe). #62 (redis) is a separate, pre-existing issue and is untouched here."

### Rule: A disclosed shortfall reads as information, not apology

A limitation gets stated plainly, with the evidence that bounds it — not hedged or apologized for.

- Wrong: "Sorry, I didn't have time to test this against real Postgres, I hope that's ok."
- Right: "Tested against SQLite rather than Postgres since I don't have Docker locally; the control run in the PR shows why the substitution still demonstrates the fix."

## Things I never post

- A guess presented as a finding.
- A fix or a timeline before I've actually investigated.
- Calling something "obviously" a duplicate or invalid without checking first.
- Copying someone else's repro text as if it were mine.
- A delivery date for when a PR will be ready.
- An approach stated as certain when it's still my best plan, not a verified result.
- A PR description claiming coverage the evidence doesn't actually show.
- An apologetic tone around a disclosed limitation instead of a plain statement of it.

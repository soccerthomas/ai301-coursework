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

## Things I never post

- A guess presented as a finding.
- A fix or a timeline before I've actually investigated.
- Calling something "obviously" a duplicate or invalid without checking first.
- Copying someone else's repro text as if it were mine.

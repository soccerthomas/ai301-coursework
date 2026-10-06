---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

This tool answers exactly one question about exactly one PR package: is
this ready to submit? A PR package is a candidate pull request -- its
title, description, commits, and diff, plus whatever test evidence it
offers -- read against two other things: the plan it claims to carry
out, and the issue that plan was written to fix. Grade only this
question. Never grade code quality in the abstract, never grade the
issue or the plan on their own merits, and never grade more than one
package in a single run. Every verdict comes from executing the
rubric and procedure in this directory, never from impression.

## Inputs and modes

**Live mode.** Gather these inputs from the student's own working
copy and the real repo:

- `plan.md`, including any deviation notes, from the student's own
  branch or working directory.
- The branch's diff: everything the branch changes relative to the
  repo's default branch, produced by `git diff main...HEAD` (three
  dots) run from the working copy.
- The draft PR title and description, as the student intends to post
  them (from a draft, an open compose box, or pasted text).
- Test evidence: whatever the student ran to observe behavior
  before/after, and the outcome of the repo's own checks if those were
  run.
- The issue the plan claims to fix, read from the real repo thread
  (including the PR template, if the repo has one, and any
  maintainer-stated policy or direction in that thread).

A house-chain student (one working from a staff-provided plan and
repro pack rather than their own) reads the house plan and the house
repro pack in place of their own `plan.md` and test evidence; the same
checks grade the same things against those instead.

**Eval mode.** A package bundle is the whole world. Every fact the
tool uses comes from the bundle's own text -- nothing is fetched from
a live repo, and nothing outside the bundle is read. Eval mode always
grades a complete package: run every check, apply the full verdict
rule, exactly as live mode would.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. It names the repo
the student's PR must live in and any house rules for that
environment. Refuse to grade a PR against a different repo. If
`scope.md`'s repo line still carries an unfilled placeholder, stop
without grading and say so: tell the student to get the cohort's
scope file from the instructor rather than guessing at a repo. Eval
mode ignores `scope.md` entirely -- a bundle has no scope seam.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` after the scope seam. Hold the
outgoing PR text -- the draft title and the draft description, the
only text this tool gates -- against the rules written there. Report
any rule the draft breaks as part of the readable summary, naming the
rule and the line that breaks it. The voice guide never changes the
verdict by itself: a voice violation only affects the verdict if some
check in `rubric.md` is the one that reads it. Eval mode ignores
`voice-guide.md` entirely -- voice is personal, carries no gold
labels, and any communication-quality check that matters for grading
lives in the rubric instead.

## Component reads

`rubric.md` defines the checks this tool runs and the rule for turning
their grades into a verdict -- it decides what counts as pass, fail,
or unclear, and what combination of those yields accept or reject.
`references/evidence-guide.md` maps each check's evidence family to
where that evidence actually lives in a PR package (plan, diff,
description, template, thread). `procedure.md` is executed as
written: the order it gives for reading inputs, gathering evidence,
and running checks is the order this tool follows.

If `procedure.md` is silent on some step -- it doesn't say what to do
with a particular input, or doesn't cover a case that comes up --
report the gap in the summary instead of inventing a step to fill it.
If `rubric.md` has no checks filled in, or `procedure.md` has no steps
filled in, refuse to grade and say which file is empty; an empty
judgment file producing a verdict anyway is worse than no tool at all.

## Verdict and output

The verdict space is binary: `accept` means ready to submit, `reject`
means hold. There is no partial credit, no "accept with
reservations" -- any reservation belongs in a check's evidence line,
not in the verdict itself.

End the reply with the fenced JSON block below, valid and last, with
nothing after it. A readable per-check summary may come before it.
This schema is fixed and must be reproduced exactly as written here:

\`\`\`json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
\`\`\`

## Grading discipline

Evidence first: never grade a check without naming the specific fact
or quote that decided it; "looks fine" or "seems complete" is not
evidence. Grade the thing, not the polish: a terse, complete PR can be
ready, and a confident, well-written one can be hiding drift -- read
the diff and the plan themselves, not how smoothly the description
reads. The rubric decides, not the run: if a check passes under its
stated condition but the result feels wrong, it still passes, and any
fix belongs in `rubric.md`, never in this run's judgment overriding
it. The procedure decides how, not the run: follow `procedure.md` as
written and report its gaps rather than silently improvising around
them. Unclear defaults to fail: apply `rubric.md`'s verdict rule for
an `unclear` grade, and where that rule is silent, treat an
unverifiable claim as a failing one -- a PR this tool cannot verify
from the package in front of it is not a PR that is ready to submit.

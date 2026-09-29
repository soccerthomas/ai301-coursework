# Procedure: how this skill grades a plan package

## Read order

1. Read the issue itself in full: title, body, labels, and — critically — any maintainer comments or draft patch/suggested direction already in the thread.
2. Read the repro evidence (the repro comment or repro-evidence block) in full, before looking at the plan. Note down the exact observed behavior it pins down: the specific artifact (error message, line, control-run result) that demonstrates the bug.
3. Read the candidate plan in full: diagnosis, scope, files/approach, test plan, risks.
4. Read the plan comment text last, checked against what was noted from steps 1 and 2.

This order matters because the plan's diagnosis can only be judged against evidence already pinned down independently in step 2 — reading the plan first risks anchoring on its own framing instead of the evidence.

## Evidence gathering

- diagnosis_grounded: from step 2's notes, extract the exact artifact/behavior the repro evidence demonstrates; from the plan, extract its stated cause verbatim; compare directly.
- scope_bounded: extract the plan's explicit in-scope/not-in-scope lines and its file list verbatim.
- executable_by_stranger: extract the plan's approach/files section verbatim.
- test_plan_observable: extract the plan's test plan section and the repro evidence's steps/artifacts side by side.
- honest_uncertainty: extract the plan's risks/unknowns section verbatim; if the section is missing entirely, that is evidence for a fail, not a reason to skip the check.
- comms_thread_aware: pull the thread's maintainer comments (or note their absence) and the repo-facts block's contribution policy and AI-use policy.

## Check execution

Grade in this order: diagnosis_grounded, scope_bounded, executable_by_stranger, test_plan_observable, honest_uncertainty, comms_thread_aware. Each check is graded independently from the evidence gathered above — a failure on diagnosis_grounded does not excuse skipping the later checks. If evidence for a check is genuinely absent from the package (a section never appears), grade that check `fail` directly rather than `unclear`; `unclear` is reserved for cases where the section exists but the evidence inside it is ambiguous. Once evidence has been extracted in the gathering stage, a check may be graded without re-reading the whole package.

## Verdict assembly

Apply the rubric's verdict rule: accept only if all six required checks pass; any fail or unclear on a required check means reject. In the output, quote the specific evidence that decided the verdict — for a reject, the plan's own words next to the evidence they contradict or fail to address; for an accept, a one-line pointer to what each check confirmed.

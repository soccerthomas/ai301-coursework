# Procedure: how this tool grades a PR package

## Read order

1. Read the plan in full first (scope, approach, test plan, risks, and any deviation notes added during the build). This is the fixed reference everything else gets checked against.
2. Read the PR diff next: every changed file and hunk, noted against the plan's stated boundary.
3. Read the PR description, noting its claims about what changed and what test evidence it presents.
4. Read the repo's PR template and CONTRIBUTING.md (including any AI-use policy).
5. Read any maintainer comments on the issue or PR thread last.

This order matters because the plan is the fixed reference: reading the diff or description first risks grading the description's own framing instead of checking it independently against the plan.

## Evidence gathering

- plan_fidelity: extract the plan's in-scope/not-in-scope lines and any deviation notes; extract the diff's file list; extract the description's "what this changes" claims; compare all three side by side.
- test_evidence: extract the plan's test plan section; extract the description's test-evidence section (before/after); note whether the repo's own checks' outcome is shown or only asserted.
- diff_quality: extract the full diff; flag any file or hunk not accounted for by the plan_fidelity mapping as a debris candidate.
- standards_comms: extract the repo's PR template section headers; extract the description's content under each; extract the AI-use policy; extract any maintainer comments.

## Check execution

Grade in this order: plan_fidelity, test_evidence, diff_quality, standards_comms. Each check is graded independently from the evidence already gathered above -- a failure on plan_fidelity does not excuse skipping the later checks. If evidence for a check is genuinely absent from the package (e.g. no test-evidence section at all), grade that check `fail` directly rather than `unclear`; `unclear` is reserved for evidence that exists but is ambiguous to interpret. Once evidence has been extracted in the gathering stage, a check may be graded without re-reading the whole package.

## Verdict assembly

Apply the rubric's verdict rule: accept only if all four required checks pass; any fail or unclear on a required check means reject. When more than one check fails, quote the first-failing required check in rubric order (plan_fidelity, test_evidence, diff_quality, standards_comms) in the output, with the specific evidence that tripped it.

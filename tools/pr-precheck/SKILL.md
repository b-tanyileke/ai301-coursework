---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer exactly one question about one PR package: is this pull request
ready to submit?

A PR package contains the candidate title and description, commit list,
diff, and test evidence. Read those artifacts against the accepted plan
and its deviation notes, the issue that plan belongs to, the issue's
reproduction evidence, and the repository's stated contribution
requirements. Do not grade unrelated work or more than one package in
a run.

## Inputs and modes

Determine the mode before gathering evidence.

In live mode, read:

- the supplied issue URL and its current thread;
- `plan.md`, including its `## Deviations` section;
- the committed branch diff relative to the repository's default
  branch, produced from the working copy with `git diff main...HEAD`;
- the PR title and description in `pr_draft.md`;
- the captured command output in `test_evidence.md`;
- `.github/PULL_REQUEST_TEMPLATE.md`;
- `docs/CONTRIBUTING.md` and any other repository policy the template
  or contributing guide makes applicable.

Treat `git diff main...HEAD` as the candidate PR's code. Do not silently
include uncommitted working-tree changes in the grade. If a required
live input cannot be read, grade its affected checks as the rubric
directs instead of inventing its contents.

For a house-chain submission, read the supplied house plan and house
reproduction pack in place of the student's own posted plan and
reproduction comment. Apply the same rubric and procedure.

In eval mode, the supplied package bundle is the whole world. Read its
issue context, thread highlights, repo facts, accepted plan context,
candidate PR title and description, commits, diff, and test evidence.
Do not fetch GitHub, inspect a working copy, or use outside knowledge.
Run every rubric check and apply the complete verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before any other package source. Confirm
that the candidate PR targets the repository named on its `Repo:` line
and apply every house rule in that file.

If the `Repo:` line still contains a bracketed placeholder, stop
without grading and tell the student to fill it with the section's Path
Review repository. Refuse to grade work targeting any other
repository. Never guess the intended scope.

Ignore `scope.md` entirely in eval mode.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` and check both the draft PR title
and description against it. Report each broken rule by name, with the
draft language that triggered it and a concise correction.

Voice-guide findings do not change the verdict unless a rubric check
explicitly uses the same underlying evidence. Do not turn a personal
style preference into an unstated grading requirement.

Ignore `voice-guide.md` entirely in eval mode.

## Component reads

Read `rubric.md` for the check names, evidence sources, pass
conditions, weights, and verdict rule. Read
`references/evidence-guide.md` for the exact location and quality
standard of each evidence family. Execute `procedure.md` as written to
gather evidence, grade every check, and assemble the verdict.

If `rubric.md` has no substantive checks or `procedure.md` has no
substantive steps, refuse to grade and identify the empty component.
Do not invent a replacement rubric or procedure at runtime.

If the procedure is silent about a necessary operation, report the gap
in the readable summary. Do not silently improvise around it.

## Verdict and output

Use only two verdicts:

- `accept`: ready to submit;
- `reject`: hold before submitting.

Before the final JSON, provide a concise per-check summary. In live
mode, include any voice-guide findings after the check summary.

End the reply with the valid fenced JSON block below and write nothing
after it. Preserve the schema exactly. Use the PR URL when one exists;
for a live draft that has not been opened, use the supplied issue URL
as the package identifier.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Lead with evidence. Give every check a concrete fact, quotation,
  changed path, diff hunk, command result, or explicitly named missing
  item. "Looks ready" and "tests pass" are not evidence.
- Grade the outcome, not the polish. A terse PR can pass, while a
  detailed PR can fail if its diff or evidence contradicts it.
- Let `rubric.md` decide the grades and verdict. Do not add requirements
  because a package feels incomplete.
- Let `procedure.md` decide how evidence is gathered and compared.
  Report procedural gaps instead of hiding them.
- Treat an honest, bounded deviation according to the rubric rather
  than assuming every departure is silent drift.
- Treat `unclear` according to the rubric's verdict rule. If that rule
  is silent, count an unverifiable required check as failing.
- Never replace missing package evidence with assumptions, likely
  outcomes, or facts from another submission.

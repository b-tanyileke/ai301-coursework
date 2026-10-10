# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

**Where it lives**

- In an eval package, read the accepted plan in the Plan context block,
  including its scope, named files or areas, implementation approach,
  exclusions, test plan, deviations, and deferrals. Compare it with the
  candidate PR's complete unified diff, commit list, title,
  description, changes list, testing claims, and reviewer notes.
- In live mode, read `plan.md`, including `## Deviations`, beside the
  committed output of `git diff main...HEAD`. Compare both with the
  title and description in `pr_draft.md`. Use the issue thread to
  identify the requested behavior and any explicit maintainer
  direction.
- Treat the diff as the implementation record. Treat the description
  as a claim about that record; it does not override what the diff
  contains.

**What good looks like**

Every substantive diff change is inside the accepted plan or an
explicit deviation that states what changed and why. Planned work is
present or honestly narrowed or deferred in both the plan and
description. The title and description state exactly what the diff and
evidence support. Silent extra work, silently missing work, and claims
of fidelity contradicted by the diff are failures.

An honest, bounded shortfall is not automatically a failure. It passes
when the remaining change is still coherent and useful, the deviation
is recorded where the package requires it, and the description does not
claim the deferred outcome.

In eval mode, the frozen bundle is the complete world. Do not infer a
contradiction from possible behavior in callers, files, or runtime
paths the bundle does not show. A disclosed boundary fails only when
the visible package evidence directly contradicts it.

## Test evidence (harness category: not-tested)

**Where it lives**

- In an eval package, read the original reproduction and test plan in
  the Plan context block. Compare them with the candidate PR's Test
  evidence, changed or added tests in the diff, testing claims, and
  repository-required checks from Repo facts.
- In live mode, read the posted reproduction in the issue thread, the
  test plan in `plan.md`, the commands and output in
  `test_evidence.md`, the test changes in `git diff main...HEAD`, the
  testing section in `pr_draft.md`, and the required checks in the PR
  template and `docs/CONTRIBUTING.md`.
- In an eval bundle, a named command plus a concrete observed error,
  value, behavior, status, or test count is visible evidence even when
  the frozen package presents the result concisely or as an annotation.
  A bare statement such as "passes" or "works" without naming the
  observable result is not decisive.
- In live mode, use the actual captured command output in
  `test_evidence.md`; a note describing output that was not captured
  does not replace it.

**What good looks like**

Issue-specific evidence exercises the behavior the code changes. It
shows an observable before result and post-change result, or an
equally decisive equivalent, and states what successful output should
be. Controls and regression tests demonstrate that the fixed path is
covered without replacing the reproduction itself.

Every applicable planned and repository-required check has a named
invocation and specific outcome. In an eval package, a concise result
with a test count or explicit clean/pass/fail status is sufficient when
the package does not contradict it. In live mode, preserve the actual
command output. A failing or unavailable check is still honest evidence
when reported with its result and reason; hiding it or saying only
"tests pass" without identifying the check is not. A result that
demonstrates a relevant regression or mandatory-policy violation still
prevents readiness.

## Diff quality (harness category: unreviewable)

**Where it lives**

- In an eval package, inspect every file and hunk in the candidate
  unified diff and read the complete commit list. Use the plan only to
  distinguish necessary work from unrelated changes.
- In live mode, inspect the full committed `git diff main...HEAD`, its
  `--stat`, changed-file list, and branch commits. Do not substitute the
  working-tree diff for the committed PR diff.
- Look inside changed tests as carefully as production code. Debug
  debris and unrelated churn can appear in either.

**What good looks like**

The intended fix is visible without unrelated material competing for a
reviewer's attention. There are no debug prints, commented-out
experiments, dead helpers, unexplained TODOs, generated debris,
drive-by refactors, unrelated options, or broad formatting churn.
Necessary imports, fixture updates, or mechanical changes remain
acceptable when their relationship to the planned change is clear.

Commit wording may be imperfect without making a clean diff
unreviewable. The deciding evidence is what the complete diff asks a
maintainer to review.

## Standards and comms (harness category: standards-wall)

**Where it lives**

- In an eval package, read Repo facts for the repository's PR template,
  checklist, contribution requirements, required artifacts, explicit
  maintainer directions, and AI-use policy. Compare them with the
  candidate title, description, checklist claims, diff, and disclosure.
- In live mode, read `.github/PULL_REQUEST_TEMPLATE.md`,
  `docs/CONTRIBUTING.md`, any policy those files reference, the issue
  thread, `scope.md`, the branch context, `pr_draft.md`, and the
  committed diff.
- Determine the scope of an AI policy before applying it. Require
  disclosure when the policy covers the candidate PR or all AI use; do
  not invent one when the repository states none.
- In eval mode, treat the candidate package as AI-assisted whenever an
  applicable policy requires disclosure. In live mode, include the AI
  assistance used for the implementation, analysis, and PR drafting.

**What good looks like**

The PR uses the repository's template with real, truthful content,
names or closes the correct issue when required, and responds to every
applicable checklist and contribution requirement. A checklist response
may report completion, give a valid not-applicable reason, or explain
why no diff is expected. Do not require an artifact for an item that is
validly non-applicable. Required applicable tests, documentation,
changelog entries, or other artifacts must appear in the diff.

The PR follows explicit maintainer and house rules and includes a
plain, own-words AI-use disclosure whenever required. Boilerplate,
blank sections, unsupported checked boxes, a claimed artifact absent
from the diff, or a missing required disclosure fails.

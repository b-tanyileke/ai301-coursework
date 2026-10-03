# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

<!-- Where the plan states its cause, and where the repro evidence
pins down the behavior that cause must explain. What it means for a
diagnosis to follow from the evidence rather than contradict or
ignore it. -->

**Where it lives**

- In an eval package, find the behavior to explain in the Issue section and the Repro evidence section. Record the trigger, observed result, relevant artifacts, and controls. Find the candidate's proposed cause and causal explanation in the Candidate plan, then read its proposed change against that cause.
- In live mode, read the issue body and current thread for the reported behavior and maintainer findings. Use the student's posted reproduction comment as the reproduction evidence. Read the diagnosis and proposed approach in `plan.md` and the draft plan comment. Judge the package from what the drafts state and quote rather than relying on unquoted files elsewhere in the working directory.

**What good looks like**

The diagnosis explains the distinctive reproduced behavior and its controls without contradicting or ignoring them. A thread claim is supporting evidence only when it remains consistent with the reproduction. The proposed change acts on that supported failure point, or clearly explains why a narrower boundary will resolve the behavior. A workaround or symptom-level change is sufficient only when that is the outcome requested by the issue or maintainers.

## Scope

<!-- Where the plan bounds itself: the in-scope statement, the
not-in-scope line, the files or areas named. What one bounded change
looks like next to a drive-by rewrite. -->

**Where it lives**

- In an eval package, read the Candidate plan's change boundary, files or code areas, approach, exclusions, and deferrals. Compare them with the Issue and Thread highlights.
- In live mode, read the in-scope and out-of-scope commitments in `plan.md`, including the files or areas expected to change. Compare them with the issue request and any explicit maintainer direction in the current thread.

**What good looks like**

The plan describes one coherent, reviewable change that resolves the behavior it claims to fix. It excludes unrelated cleanup, redesigns, migrations, refactors, and adjacent defects. A scoped-down solution is acceptable when its boundary and deferrals are explicit and the remaining change is still useful and complete on its own. Do not require a particular heading structure or an exhaustive list of line numbers.

## Executability

<!-- Where the plan says what will actually be done: files or areas,
approach, order of work. What it means for a stranger to be able to
start executing without asking the author anything. -->

**Where it lives**

- In an eval package, read the Candidate plan's named files or code areas, chosen technical mechanism, implementation steps, and ordering. Use relevant Thread highlights and Repo facts to identify decisions that have already been made or constraints the plan must follow.
- In live mode, read the approach and file descriptions in `plan.md` against the repository documentation and current issue thread. Determine whether the plan has selected the layer and mechanism where work will begin.

**What good looks like**

A contributor familiar with the repository can begin the change without first inventing the approach, choosing among unresolved technical layers, or deciding what the fix is supposed to be. Exact line numbers and incidental coding details are unnecessary. A verification question may remain open when it is explicitly bounded and its answer would refine the implementation rather than determine the entire approach.

## Test plan

<!-- Where the plan says how success will be observed, and how that
maps onto the repro evidence's steps and artifacts. What a decisive
test plan names that a vague one does not. -->

**Where it lives**

- In an eval package, compare the Candidate plan's test plan with the Repro evidence's trigger, commands or steps, artifacts, controls, expected result, and actual result.
- In live mode, compare the test plan in `plan.md` with the student's posted reproduction comment. Look for an explicit reuse of those steps or an explained equivalent that exercises the real changed code.

**What good looks like**

The test plan names an observable post-fix result that distinguishes success from the reproduced failure. It may refer back to already-recorded reproduction steps instead of repeating every command, but it must state what will change when those steps are rerun. It also includes a focused regression check or relevant control when needed to show existing behavior remains intact. A generic full-suite run can supplement this evidence but cannot replace an issue-specific test, and statements such as “works,” “looks better,” or “feels fast” are not decisive outcomes.

## Honesty

<!-- Where claims meet uncertainty: risks, unknowns, and deviations.
How to tell stated unknowns from false confidence, and where an
honest mid-build deviation gets recorded. -->

**Where it lives**

- In an eval package, compare the Candidate plan's diagnosis, risks, unknowns, assumptions, deferrals, and any deviation statements with the Issue, Repro evidence, and Thread highlights.
- In live mode, compare those statements in `plan.md` and the draft comment with the student's posted reproduction and the current issue thread. After implementation, also read the `## Deviations` section for changes between the posted plan and the build.

**What good looks like**

The plan claims only what the available evidence supports. Material unknowns, unverified assumptions, risks, and deferred work are stated when they could affect the proposed change or its verification. A plan does not need a ceremonial risks section when no material uncertainty is apparent. An explicit bounded deferral is honest planning, while presenting an unresolved decision as settled or hiding a changed implementation behind the original plan is not.

## Comms

<!-- Where the words meet the thread and the repo: the plan comment
read against the issue's maintainer signals (thread highlights, or
the live thread) and against the repo-facts block's stated templates,
contributing asks, and contribution policy (including AI-use
disclosure requirements). What thread-aware looks like next to
boilerplate. -->

**Where it lives**

- In an eval package, read the Candidate plan comment against the Candidate plan, Issue, Thread highlights, and Repo facts. Inspect the contribution policy, templates, prior-work references, and any AI-use or disclosure requirements.
- In live mode, read the draft plan comment against `plan.md`, the current GitHub issue thread, repository contribution documentation, issue and pull-request templates, and any AI policy. Also apply the Path Review house rules from `scope.md`.
- In eval mode, treat the candidate package as AI-assisted when applying an explicit AI-disclosure policy. In live mode, a comment drafted or revised with the skill is AI-assisted. First determine whether the policy applies to issue comments, all AI use, or only code and pull requests.

**What good looks like**

The comment accurately summarizes the supported diagnosis, bounded change, and decisive verification without promising work absent from the plan. It engages explicit maintainer direction, rejected approaches, linked pull requests, and relevant prior work rather than posting boilerplate that could fit any issue. It follows every convention that applies to issue comments. Require AI disclosure only when the policy covers issue comments or all AI use; do not extend a pull-request-only disclosure rule to the issue thread. A rule requiring comments in the contributor's own words is satisfied only when the contributor has reviewed and rewritten the comment accordingly. In Path Review, another student's plan does not block the candidate, but “same approach as above” and copied reasoning are not independent plans.

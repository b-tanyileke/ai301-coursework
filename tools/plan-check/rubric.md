# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
## Checks

| Check | Evidence | Pass condition | Weight |
| --- | --- | --- | --- |
| diagnosis-grounded | The candidate plan's stated cause and causal explanation read against the issue context and the reproduction evidence's steps, observed results, artifacts, and controls. Use thread claims only where they remain consistent with that reproduced evidence. | The diagnosis explains the distinctive reproduced behavior and the relevant controls without contradicting or ignoring them. It identifies a supported failure point or mechanism rather than adopting an unsupported thread claim, guessing from proximity, or naming only the visible symptom. | required |
| change-addresses-cause | The candidate plan's proposed approach, change boundary, and named code areas read against its supported diagnosis and the issue's expected behavior. | The proposed change acts on the supported failure point, or on a narrower boundary whose relationship to that cause is explicitly justified, and would produce the requested behavior. A workaround or symptom mask passes only when the issue or maintainer direction asks for that outcome; otherwise the plan must address the supported cause. | required |
| scope-bounded | The candidate plan's in-scope and out-of-scope commitments, files or code areas, approach, and stated deferrals read against the issue and thread highlights. | The plan describes one coherent, reviewable change and excludes unrelated refactors, migrations, redesigns, cleanup, or adjacent defects. A deliberately scoped-down solution passes when its boundary and deferrals are explicit and it still resolves the behavior the plan claims to fix. | required |
| executable-by-stranger | The candidate plan's files or code areas, chosen mechanism, work sequence, and implementation decisions, read with the repository facts and thread directions needed to begin the change. | A contributor familiar with the repository could start the change without first choosing the technical layer, inventing the approach, or resolving a material decision the plan leaves open. Exact line numbers and incidental implementation details are not required, and a bounded verification question may remain open when it does not decide the approach. | required |
| test-decisive | The candidate plan's test plan read against the reproduction evidence's trigger, commands or steps, artifacts, controls, and expected behavior. | The test plan reuses the reproduction or names an equivalent check and states an observable post-fix result that distinguishes success from the reproduced failure. It also identifies a focused regression check or relevant control when needed to show existing behavior is preserved. A generic full-suite run or phrases such as "works," "looks better," or "feels fast" without a measurable issue-specific outcome do not pass. | required |
| claims-honest | The candidate plan's diagnosis, risks, unknowns, deferrals, and any deviation statements read against the issue, reproduction evidence, and thread highlights. | The plan claims no more certainty or coverage than the evidence supports. Material unknowns, unverified assumptions, risks, and deliberately deferred work are identified where they affect the proposed change or its verification. A separate risks section is not required when no material uncertainty is apparent, and an explicit bounded deferral is not itself a failure. | required |
| comment-aligned | The candidate plan comment read against the candidate plan, issue context, thread highlights, repo-facts contribution policy, applicable AI-use or disclosure rules, and, in live mode, the Path Review house rules. | The comment faithfully summarizes the supported diagnosis, bounded change, and decisive verification without promising work the plan does not contain. It engages explicit maintainer direction and relevant prior work, and follows every convention that applies to issue comments. Require AI disclosure only when the stated policy covers issue comments or all AI use; do not extend a pull-request-only rule to the issue thread. In Path Review live mode, another student's plan is not a blocker, but the candidate may not substitute or copy that plan for their own. | required |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->

Accept only when every required check passes. A `fail` or `unclear`
grade on any required check produces `reject`. An `unclear` grade means
the evidence needed to apply the check is genuinely absent from the
package, not that the evaluator skipped looking for it. Preferred
checks, if any are added later, do not change the verdict.

# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides the order (issue first? repro evidence first?) and says why
the order matters for the checks that come later. -->

1. Identify the mode before reading the package.
   - In live mode, read `scope.md` first. Confirm that the issue belongs to the repository named there and record the Path Review house rules. If the repository line is still a placeholder or the issue is outside that scope, stop without grading.
   - In live mode, also read `voice-guide.md` and record its rules for the later comment review.
   - In eval mode, ignore `scope.md` and `voice-guide.md`; the supplied bundle is the complete source of evidence.
2. Read `rubric.md` and record every check, its evidence source, its pass condition, its weight, and the verdict rule. Then read `references/evidence-guide.md` so each evidence family has a known location before gathering begins.
3. Read the issue context, repo facts, and thread highlights before reading the candidate plan. Record:
   - the reported trigger and expected behavior;
   - explicit maintainer findings, requested directions, rejected approaches, and relevant prior work;
   - applicable contribution, communication, and AI-use policies.
4. Read the reproduction evidence next. Record the tested trigger, observed behavior, artifacts, controls, supported failure point, and any stated limitations. Establish this evidence as the baseline the plan must explain.
5. Read the entire candidate plan without grading it yet. Record its diagnosis, proposed change, boundaries, files or code areas, implementation approach, test plan, risks, unknowns, deferrals, and deviations.
6. Read the candidate plan comment last. Record what it tells maintainers about the diagnosis, scope, approach, verification, prior work, and any required disclosure.
7. Keep this order so confident wording in the plan or comment cannot replace or redefine what the issue and reproduction evidence actually establish.

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which page or
thread location per your evidence guide) to pull the fact from, and
what to record. A complete procedure leaves no check whose evidence an
executor would have to hunt for. -->

1. Create one evidence record for each rubric check. Use the locations and observable conditions in `references/evidence-guide.md`; do not rely on memory or general impressions.
2. For `diagnosis-grounded`, place the candidate's stated cause beside the reproduction's trigger, observed result, artifacts, and controls. Record the facts that support it and any fact that contradicts or rules it out.
3. For `change-addresses-cause`, connect the proposed mechanism and code area to the supported failure point. Record how the change is expected to produce the issue's requested behavior and whether it is a root-cause change, a justified boundary change, or an unsupported symptom workaround.
4. For `scope-bounded`, list the claimed outcome, in-scope work, exclusions, files or areas, and deferrals. Compare that list with the issue and thread, and record any unrelated cleanup, redesign, migration, refactor, or adjacent defect included in the plan.
5. For `executable-by-stranger`, record the chosen technical layer, mechanism, starting files or areas, work sequence, and remaining decisions. Mark which remaining decisions are merely implementation details and which would require the executor to invent the approach.
6. For `test-decisive`, map each proposed test to the reproduction trigger or an explained equivalent. Record the current failing observation, the expected post-fix observation, and any focused regression check or control that protects existing behavior.
7. For `claims-honest`, compare the plan's certainty, risks, assumptions, unknowns, deferrals, and deviations with the issue, reproduction evidence, and thread. Record unsupported claims, hidden uncertainty, or boundaries the plan states honestly.
8. For `comment-aligned`, compare the candidate comment with the plan, issue, thread directions, prior work, and applicable repository policies.
   - Determine whether any AI rule applies to issue comments, all AI use, or only code and pull requests before judging disclosure.
   - In eval mode, treat the candidate package as AI-assisted when an applicable policy requires disclosure.
   - In live mode, account for the AI assistance used to draft or revise the comment.
   - Apply the Path Review rule that another student's plan is not a blocker, while still requiring an independent plan.
9. In eval mode, gather evidence only from the bundle. Do not fetch live information or fill gaps from outside knowledge.
10. In live mode, gather issue-side evidence only from the locations named in the evidence guide. Grade the drafts from what they state and quote; do not silently repair them with unquoted information from the student's working directory.
11. When required evidence is absent, write `missing` in that check's evidence record. Do not substitute a guess.

## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

1. Grade the checks in this dependency order:
   1. `diagnosis-grounded`
   2. `change-addresses-cause`
   3. `scope-bounded`
   4. `executable-by-stranger`
   5. `test-decisive`
   6. `claims-honest`
   7. `comment-aligned`
2. For each check, reread that rubric row's Evidence and Pass condition, then use only the corresponding evidence record.
3. Assign `pass` only when the gathered evidence satisfies the complete pass condition.
4. Assign `fail` when the gathered evidence contradicts the pass condition or directly shows that the condition is not met.
5. Assign `unclear` when evidence required to decide the check is genuinely absent or too ambiguous to support either pass or fail. Do not use `unclear` merely because the check is difficult.
6. Write one evidence line for every grade.
   - Quote or closely identify the package fact that decided the check.
   - For a failed relationship check, include both sides of the conflict when possible, such as the plan's claimed cause beside the reproduction control that rules it out.
   - For missing evidence, name what is missing instead of writing a general statement such as “not enough detail.”
7. Judge outcomes, not formatting. Do not fail a plan for being short, lacking a preferred heading, omitting line numbers, or using a different section order when the substantive pass condition is met.
8. Do not add requirements that are absent from the rubric. If a package feels wrong but passes the written condition, give the grade the rubric requires and note the tension for later rubric revision.
9. Grade every rubric row exactly once. Do not allow one failed check to skip the remaining checks.
10. In live mode, compare the draft comment with `voice-guide.md` after the rubric checks. Report any broken voice rule separately; it does not change the verdict unless a rubric check explicitly makes it evidence.

## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check. A
complete procedure produces the same verdict from the same grades,
every time. -->

1. Confirm that every rubric check has a `pass`, `fail`, or `unclear` grade and a one-line evidence record.
2. Apply the verdict rule exactly:
   - return `accept` only when every required check passes;
   - return `reject` when any required check is `fail` or `unclear`;
   - preferred checks, if any are later added, never change the verdict.
3. For a rejection, identify each required check that caused the rejection. Quote or name the submission evidence that decided those checks rather than replacing it with general advice.
4. Before the JSON block, write a short readable summary with one line per check. In live mode, add any voice-guide warning after the check summaries and before the JSON.
5. Emit the final result as a valid fenced JSON block using the output contract in `SKILL.md`. Preserve the exact rubric check names and use only `pass`, `fail`, or `unclear` for grades and `accept` or `reject` for the verdict.
6. Make the fenced JSON block the final content in the response. Write nothing after it.

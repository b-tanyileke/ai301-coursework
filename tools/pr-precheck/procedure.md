# Procedure: how this tool grades a PR package

## Read order

1. Identify the mode.
   - In live mode, read `scope.md` first, validate the target
     repository, and record its house rules. Stop without grading if
     the repository line is unfilled or the target is outside scope.
   - In live mode, read `voice-guide.md` and record its rules for the
     final title-and-description review.
   - In eval mode, ignore both files and use only the supplied bundle.
2. Read `rubric.md`. Record every check in table order, its evidence
   source, pass condition, weight, and the verdict rule.
3. Read `references/evidence-guide.md` and record where each evidence
   family lives in the current mode.
4. Read the issue context, thread highlights, and repository facts
   before reading the candidate PR. Record:
   - the issue number, trigger, observed failure, and requested
     behavior;
   - explicit maintainer findings, constraints, and rejected
     approaches;
   - the PR template, contribution requirements, and applicable AI
     policy.
5. Read the accepted plan before the diff. Record:
   - the intended outcome and implementation approach;
   - in-scope and out-of-scope work;
   - named files or code areas;
   - the issue-specific test plan and repository checks;
   - every deviation, deferral, or known shortfall.
6. Establish the original reproduction baseline from the plan context
   or live issue thread. Record the exact trigger, before result,
   expected post-change result, and relevant controls.
7. Read the complete commit list and unified diff without reading the
   candidate description first. Record every changed file, each
   substantive hunk, and any debris or unrelated work.
8. Read the complete test-evidence block. Record each command or step,
   its visible output, the code path exercised, and whether it is
   before evidence, after evidence, a focused regression, or a
   repository check.
9. Read the candidate title and description last. Map each factual
   claim and checklist item to the plan, diff, test evidence, or
   repository rules.
10. Preserve this order so the candidate's confident wording cannot
    redefine the accepted plan, hide an unrelated hunk, or substitute
    for actual output.

## Evidence gathering

1. Create one evidence record for each rubric check. Use the locations
   in `references/evidence-guide.md`; do not grade from memory or an
   overall impression.
2. For `diff-matches-plan`:
   - list every file and substantive behavior promised by the plan;
   - list every file and substantive behavior present in the diff;
   - list every plan deviation or deferral;
   - identify extra or missing work not accounted for by a deviation.
3. For `description-matches-change`:
   - list the title and description's claims about the issue, behavior,
     files, tests, completeness, and deviations;
   - map each claim to a supporting diff hunk, evidence result, or plan
     note;
   - record contradictions, unsupported claims, and material omissions.
   - in eval mode, treat the frozen bundle as complete and do not infer
     contradictions from unshown callers, files, or runtime behavior.
4. For `reproduction-proven`:
   - place the original trigger and before result beside the candidate's
     before-and-after evidence;
   - verify that the after run exercises the changed path;
   - record the expected-after result and whether the observed output
     demonstrates it.
   - in eval mode, accept concise annotations when they state a
     concrete observed error, value, output, or behavior tied to the
     command; in live mode, require captured output.
5. For `required-checks-reported`:
   - list every focused, regression, suite, lint, build, type, or other
     check required by the plan or repository;
   - record the command or invocation and visible outcome for each;
   - distinguish an honestly reported failure from a hidden or merely
     asserted result.
   - in eval mode, accept a named command or check with a specific
     result, count, or clean/pass/fail status; do not require a complete
     terminal transcript.
6. For `diff-reviewable`:
   - inspect every changed file and hunk;
   - record debug output, commented-out attempts, dead code, TODOs,
     formatting-only churn, generated debris, broad renames, or
     unrelated features;
   - record why any necessary mechanical hunk belongs to the change.
7. For `template-complete`:
   - list every applicable template section, checklist item, and
     required artifact;
   - map each one to real description content or a changed file;
   - verify that checked boxes and not-applicable statements are true.
   - treat a stated not-applicable or no-diff-needed response as
     sufficient when its reason is valid and nothing in the package
     contradicts it.
8. For `policy-compliant`:
   - record each applicable contribution, disclosure, branch, and
     maintainer requirement;
   - locate the package evidence satisfying it;
   - in eval mode, treat the candidate package as AI-assisted when an
     explicit AI-use policy applies;
   - in live mode, account for the AI assistance used in preparing the
     code and PR text.
9. In eval mode, gather evidence only from the bundle.
10. In live mode, gather evidence only from the named files, committed
    branch diff, issue thread, PR template, contribution documentation,
    and applicable repository policies.
11. When required evidence is absent, record exactly what is missing.
    Do not replace it with a guess.

## Check execution

1. Grade the checks once each in rubric order:
   1. `diff-matches-plan`
   2. `description-matches-change`
   3. `reproduction-proven`
   4. `required-checks-reported`
   5. `diff-reviewable`
   6. `template-complete`
   7. `policy-compliant`
2. Before grading a check, reread that rubric row's Evidence and Pass
   condition. Use only its prepared evidence record.
3. Assign `pass` only when the complete pass condition is satisfied.
4. Assign `fail` when the evidence directly contradicts the pass
   condition or shows that it is not met.
5. Assign `unclear` only when evidence needed to decide is genuinely
   absent or irreducibly ambiguous. Do not use it because a comparison
   is difficult.
6. Write one deciding evidence line for every grade.
   - Identify or quote the concrete package fact.
   - For a mismatch, name both sides, such as the plan boundary and the
     extra diff hunk.
   - For missing evidence, name the absent command, output, section,
     artifact, or disclosure.
7. Judge substance, not formatting. Do not fail a package for being
   terse, using different headings, having several commits, or lacking
   stylistic polish when the substantive condition passes.
8. Do not invent checks. If a package feels wrong but passes the
   written rubric, assign the grade required by the rubric and report
   the tension for later rubric revision.
9. Grade all checks even after finding a failure.
10. In live mode, compare the title and description with
    `voice-guide.md` after completing the rubric checks. Report voice
    findings separately unless the same underlying problem also fails a
    rubric check.

## Verdict assembly

1. Confirm that every rubric check has exactly one `pass`, `fail`, or
   `unclear` grade and one deciding evidence line.
2. Apply the rubric's verdict rule:
   - return `accept` only when every required check passes;
   - return `reject` when any required check is `fail` or `unclear`;
   - preferred checks, if later added, do not change the verdict.
3. For a rejection, use the first failing or unclear required check in
   rubric order as the primary deciding check. Still report every other
   failed or unclear check.
4. Write a concise readable summary with one line per check. In live
   mode, place voice-guide findings after those lines.
5. Emit the fixed JSON schema from `SKILL.md`. Preserve the rubric check
   names exactly and use only the allowed grade and verdict values.
6. Make the fenced JSON block the final content in the response. Write
   nothing after it.

# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
| --- | --- | --- | --- |
| claim-specific | The candidate claim comment read against the issue title, body, trigger, and reported behavior. | The claim identifies the specific issue behavior or trigger, states that the contributor will investigate, reproduce, or report it, and avoids guaranteeing a fix, merge, or completion date. Any statement that reproduction is already complete must be supported by the reproduction report. | required |
| environment-recorded | The reproduction report's environment record read against the issue context and the repository facts, including bug-report requirements. | The report identifies the tested code state or release and the relevant operating system, tool, language, or runtime versions needed to place the result. It includes any additional environment fact explicitly required by the repository. Material differences from the issue's environment are stated. | required |
| steps-rerunnable | The preparation and execution steps, commands, inputs, paths, and starting state in the reproduction report. | A stranger using the recorded environment can perform the same setup and test without guessing any choice that could change the observed behavior. Commands, trigger inputs, and ordering that are material to the issue need to be supplied. Details may be summarized or left arbitrary when they do not affect the trigger and the report makes that clear. | required |
| target-behavior-shown | The report's output excerpts, logs, screenshots, exit codes, or other artifacts read against the issue's exact trigger and reported behavior. | For a claimed reproduction, the artifacts directly show the distinctive behavior described by the issue using the reported trigger or an explained equivalent. For an explicitly stated cannot-reproduce result, the artifacts show a genuine attempt to exercise the reported trigger and show the observed nonoccurrence. The report also states any environment differences, input limitations, or untested conditions that could explain the result. A different or adjacent failure presented as a successful reproduction does not pass. | required |
| outcome-supported | The claim comment and the report's expected, actual, analysis, and conclusion statements read against the included artifacts. | The package states exactly what happened and no more. A reproduced or cannot-reproduce conclusion passes only when it agrees with the commands and artifacts shown; unsupported certainty, omitted contradictory results, or calling a different failure the reported bug does not pass. | required |
| repo-conventions-followed | The repository-facts block, contribution policy, issue or bug-report template, stated AI-use policy, and both candidate comments. | The comments satisfy every applicable communication or disclosure requirement stated by the repository, including required AI-assistance disclosure. When the repository states no such requirement, the absence of a disclosure does not fail this check. | required |
| required-ai-disclosure | The repository facts' contribution or AI-use policy read against the candidate claim and reproduction comments. | First determine whether the stated disclosure rule applies to issue comments, all AI use, or only pull requests and code contributions. In eval mode, treat the candidate package as AI-assisted work. In live mode, a draft checked or revised with this skill is AI-assisted. Pass when no disclosure requirement applies to these comments, or when the comments include every required disclosure, including the tool and extent of assistance when requested. Fail when an applicable disclosure is required but missing. Do not extend a pull-request-only disclosure rule to issue comments. | required |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->

Accept only when every applicable required check passes. A fail or unclear grade on any applicable required check produces reject.

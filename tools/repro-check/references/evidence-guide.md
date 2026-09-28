# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

<!-- Where the environment record lives, and what a sufficient one
looks like against the issue's stated target. -->

**Where it lives**

- In an eval package, find the issue's target environment in the issue context and any repository requirements in the repo-facts block. Find the environment actually tested in the candidate reproduction report.
- In live mode, read the issue body and repository documentation or bug-report template for the target environment. Read the draft reproduction comment for the contributor's operating system, tested code state, relevant tool or runtime versions, installation method, and other setup facts.

**What good looks like**

The recorded environment identifies the release, tag, branch, or commit tested and the operating system and relevant language, runtime, tool, or dependency versions needed to place the result. It satisfies any additional facts explicitly requested by the repository and calls out material differences from the issue reporter's environment.

## Steps

<!-- Where the reproduction steps live, and what makes them followable
by a stranger, starting state to trigger. -->

**Where it lives**

- In an eval package, find the starting state, preparation, inputs, commands, and execution order in the candidate reproduction report. Use the issue context and repo-facts block to check whether required setup or commands were omitted.
- In live mode, read the draft reproduction comment for the exact setup and reproduction sequence. Compare it with the issue's reproducer and the repository's documented installation, build, and test instructions.

**What good looks like**

A stranger starting from the stated code state and environment can recreate the trigger and execute the test without guessing any choice that could change the result. Material commands, inputs, paths, configuration, and ordering are supplied. Details may be summarized or left arbitrary when they do not affect the behavior being tested and the report makes that clear.

## Behavior shown

<!-- Where the artifacts live (output excerpts, logs, screenshots),
and what it means for an artifact to show the issue's behavior rather
than an adjacent one. -->

**Where it lives**

- In an eval package, find terminal output, logs, stack traces, screenshots, exit codes, generated files, test results, or other artifacts in the candidate reproduction report. Compare them with the issue's exact trigger and reported behavior.
- In live mode, find those artifacts in the draft reproduction comment and compare them with the issue body, including distinctive errors, output, state changes, or missing behavior.

**What good looks like**

For a successful reproduction, the artifact itself shows that the reported trigger or an explained equivalent produced the issue's distinctive behavior. A different parse error, exception, exit code, input, or nearby failure is not the same behavior. For an honest cannot-reproduce result, the artifact shows a genuine attempt and the observed nonoccurrence, while the report identifies environmental differences, limitations, or untested conditions that might explain why the behavior did not appear.

## Honesty

<!-- Where claims and their backing meet: how to tell a report that
says exactly what happened (including an honest cannot-reproduce) from
one that claims more than its evidence shows. -->

**Where it lives**

- In an eval package, compare the candidate claim comment and the reproduction report's expected, actual, analysis, and conclusion statements with the commands and artifacts shown.
- In live mode, compare every statement in the draft claim and reproduction comments with the supplied output and the issue context. Check both positive reproduction claims and cannot-reproduce claims.

**What good looks like**

The conclusion describes only what the evidence establishes. It may say reproduced, cannot reproduce, or partially reproduced when the artifacts support that result. It does not hide contradictory output, turn an adjacent failure into confirmation, or claim certainty beyond the tested environment.

## Comms

<!-- Where the words meet the repo: the claim comment against the
issue, the comments against the repo's stated templates and
contribution policy (including AI-use disclosure requirements), and
what specific-and-honest looks like next to boilerplate. -->

**Where it lives**

- In an eval package, read the candidate claim and reproduction comments against the issue context and the repo-facts block, including contribution policies, issue templates, bug-report requirements, and AI-assistance disclosure rules.
- In live mode, read the draft comments against the GitHub issue, repository contribution documentation, issue or pull-request templates, AI policy, and the Path Review house rules in `scope.md`.
- For AI disclosure, first determine the policy's scope: whether it governs issue comments, all AI-assisted communication, or only code and pull requests. In eval mode, treat the candidate package as AI-assisted. In live mode, account for the assistance used to draft or revise the comment.

**What good looks like**

The claim identifies the particular issue behavior or trigger and states the next investigation, reproduction, or reporting step without guaranteeing a fix, merge, or deadline. Both comments follow every applicable repository communication requirement. For AI disclosure, a policy covering issue comments or all AI use passes only when the required disclosure is present, including the tool and extent when requested. A policy limited to pull requests or code does not create an issue-comment requirement. If no applicable disclosure rule is stated, silence alone is not a failure. On Path Review, another student's claim does not block the contributor, but the reproduction evidence must still be their own rather than a piggyback confirmation.

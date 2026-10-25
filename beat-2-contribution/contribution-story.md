# Contribution: error copying NVDA user settings

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1
**Student:** Tanyieleke Nkolaka
**GitHub Username:** b-tanyileke
**Issue:** https://github.com/nvaccess/nvda/issues/20118
**Pull Request:** Pending

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built and tested an issue-scout for finding and grading first-contribution candidates**

I extended my issue-selection skill into an issue-scout with my fit profile, four GitHub search queries, ranking filters, and a grading rubric. The initial scan prioritized brand-new, zero-comment issues, which made it select an issue that gained a pull request within ten minutes of creation. So I added `createdAt` to every query and excluded issues younger than two full days. Direct-grading three candidates from the course’s Issue Selection site accepted NVDA #20118 and exposed additional gaps around agent-assigned issues, maintainer attitudes toward AI-assisted work, and fixes blocked by another repository; those were recorded as future rubric improvements in scan-notes.md.

**Unit 6: Added a voice guard for upstream posts and a local NVDA repository memory file**

I modified my voice guide with rules for claims, questions, and follow-up messages, then installed a hook that checks outgoing GitHub text before Claude Code posts it. The starter hook watched for Bash commands, but my version of Claude Code (2.1.310) used its PowerShell tool on Windows, so the hook initially did not inspect those commands; setting `CLAUDE_CODE_USE_POWERSHELL_TOOL` to `0` restored the Bash path, and the wiring test stopped an unreadable `gh issue comment` command. I also created and excluded the `CLAUDE.local.md` file for NVDA with its contribution rules, test commands, coding standards, and findings from the affected subsystem.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I found this issue through the course's Issue Selection site and graded it with my issue-scout before choosing it. The scout accepted it and rated it as moderate difficulty. The repository is active, a maintainer suggested retrying the file copy, and the issue is limited to one identifiable behavior in `installer.tryCopyFile`.

This issue fits my profile because it is a focused Python bug which I can attempt on my Windows 11 pc. This will help me practice reproducing a specific file handling problem in an established codebase. The scout pointed out that though the likely code change is small, reproducing the locked-file behavior is the harder part. The scout's scope check was too strict about issues with open questions, so I replaced it with `scope-feasible` and added a difficulty note.

**Claim comment:** https://github.com/nvaccess/nvda/issues/20118#issuecomment-6385458583

**Toolkit**

- Tool: I used issue-scout to grade candidates from the Issue Selection site against my fit profile and rubric, then regraded them after applying the Unit 6 scope patch.
- What it produced: It accepted NVDA #20118, rated it as moderate difficulty, identified the likely `installer.tryCopyFile` area, and warned me about an abandoned earlier attempt and the testing evidence maintainers had requested.
- Difficulties: The original scope check treated unresolved questions like rejection reasons instead of increasing difficulty.
- What changed: I replaced `Scope is bounded` with `scope-feasible` and added straightforward, moderate, and ambitious difficulty notes.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

I tested on Windows 11 26H2 build 26300.9711 using Python 3.13.15 and an installed NVDA 2027.1.0dev build from commit `40e2df64cd2fdecdefa0b98f1056991b622c3d6e`. I initialized the repository submodules, built the source with `.\scons.bat source -j 1`, packaged it with `.\scons.bat launcher -j 1`, and installed the generated launcher. No add-ons were installed or enabled.

Preparing the environment required installing the system Python version expected by NVDA and adding the Visual Studio C++ Clang tools. The initial build failed until those dependencies were available.

**Steps to Reproduce**

1. In NVDA Settings > General, I selected "Use currently saved settings during sign-in and on secure screens" and approved UAC. The normal-copy baseline succeeded.
2. I created a harmless file in `%APPDATA%\nvda`, opened it with read/write sharing enabled, and applied a byte-range lock with PowerShell's `FileStream.Lock`.
3. I repeated the settings-copy operation while the lock remained active. NVDA displayed "Error copying NVDA user settings," and its log recorded `OSError: error 33 copying` from `installer.tryCopyFile`.
4. I released the lock without changing the file and repeated the operation. The copy succeeded.
5. I removed the test file, copied the cleaned configuration again, and verified that the test file was absent from both the user and system configurations.

I used a byte-range lock because the reporter's log contained Windows error 33, a lock violation. My earlier exclusive-open test produced error 32, a sharing violation, so it did not match the reported failure closely enough.

**Reproduction Evidence**

- **Commit showing reproduction:** The controlled test used the unchanged baseline at [`40e2df6`](https://github.com/b-tanyileke/nvda/commit/40e2df64cd2fdecdefa0b98f1056991b622c3d6e); no reproduction-specific source code was committed.
- **Screenshots/logs:** The visible error dialog said "Error copying NVDA user settings." The log showed error 33 while copying the controlled test file through `_setSystemConfig` and `installer.tryCopyFile`.
- **My findings:** `config._setSystemConfig` calls `installer.tryCopyFile` once for each configuration file. Unlike `installer.tryRemoveFile`, which retries up to six times, `tryCopyFile` has no retry loop for this failure. The same file failed while byte-range locked and copied successfully after the lock was released. This supports the reported locked-file failure path but does not identify which AIAssistant operation creates the reporter's lock or how long it lasts. I also did not test the reporter's add-on configuration or measure which unrelated files were copied before the failure.

**Toolkit**

- Tool: I used `repro-check` to grade my claim and reproduction report against the issue, repository conventions, and evidence rubric.
- What it produced: It accepted the final package after confirming that my controlled test matched the reporter's error 33, visible dialog, and failing code path.
- Difficulties: My first controlled test used an exclusive file handle and produced error 32 rather than the reporter's error 33. Preparing the NVDA build also required resolving Python and Visual Studio Clang dependencies.
- What changed: I replaced the exclusive-open test with a byte-range lock, reran the failure and recovery controls, and revised the report to distinguish the controlled reproduction from the reporter's add-on environment.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]

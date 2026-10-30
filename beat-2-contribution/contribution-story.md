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

Copying NVDA user settings for sign-in and secure screens can fail when one eligible configuration file is temporarily locked. Because the copy stops at the first failure, one locked file prevents the entire settings-copy operation from completing.

**Expected Behavior**

NVDA should copy all requested user settings and add-ons into the system configuration and report that the copy succeeded. A temporary file lock should not abort the operation if it clears during a bounded retry period, while a permanent failure should still be reported.

**Current Behavior**

`config._setSystemConfig` calls `installer.tryCopyFile` once for each eligible configuration file. A byte-range lock produces Windows error 33, which `tryCopyFile` raises as an `OSError`. The elevated `setNvdaSystemConfig` action then exits with an error, and NVDA displays “Error copying NVDA user settings.”

**Affected Components**

The affected flow is `source/config/__init__.py` through `setSystemConfigToCurrentConfig` and `_setSystemConfig`, `source/installer.py` through `tryCopyFile`, and `source/nvda_slave.pyw` through the elevated `setNvdaSystemConfig` action. Focused regression tests belong in `tests/unit/test_installer.py`.

**Toolkit**

- Tool: I used Claude Code to trace the settings-copy flow in my local NVDA checkout, then verified its findings against the current upstream source.
- What it produced: It identified the path from `config._setSystemConfig` to `installer.tryCopyFile`, the missing retry around `CopyFile`, the elevated slave’s error handling, and the existing retry pattern in `tryRemoveFile`.
- Difficulties: The initial analysis stopped before inspecting `nvda_slave.pyw`, and the issue-page output did not include the complete original report.
- What changed: I inspected the missing slave and test paths, confirmed that error 33 becomes an ordinary slave failure, and narrowed the proposed change to `tryCopyFile` and its unit tests.

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

`setSystemConfigToCurrentConfig` in `source/config/__init__.py` starts the secure-screen settings copy, using the elevated `setNvdaSystemConfig` action when needed. `_setSystemConfig` removes the previous system configuration and calls `installer.tryCopyFile` once for each eligible user-configuration file.

In `source/installer.py`, `tryCopyFile` calls the Windows `CopyFile` function only once on the initial copy path. When a source file is temporarily locked and the destination does not yet exist, the function immediately raises an `OSError`. The controlled reproduction confirmed this path with Windows error 33. By comparison, `tryRemoveFile` already uses a bounded retry interval for temporary filesystem failures.

The duration and add-on operation responsible for the reporter’s original lock are still unknown. The controlled test proves the same error code, visible failure, and code path, but it does not reproduce the reporter’s complete AIAssistant environment.


**Proposed Solution**

Add configurable retry-count and retry-interval parameters to `installer.tryCopyFile`, with defaults matching the bounded behavior used by `tryRemoveFile`. Retry the initial `CopyFile` operation when the destination does not exist, allowing a temporary source-file lock to clear before raising the existing `OSError`.

Keep the existing destination-overwrite fallback unchanged. Do not change which configuration files `_setSystemConfig` copies, the elevated-process protocol, add-on behavior, or the user-facing dialogs. Permanent failures will still be reported after the bounded attempts are exhausted.

Because `tryCopyFile` is also used by installation, user-configuration, and portable-copy paths, those callers will receive the same bounded retries and may wait about three seconds longer before a permanent failure is reported. Unit tests will cover `tryCopyFile`'s existing successful-copy and overwrite behavior.

**Implementation Plan**

1. Modify `tryCopyFile` in `source/installer.py` to make up to six initial copy attempts with a configurable 0.5-second interval. Return immediately after a successful copy and preserve the existing destination-overwrite path.
2. Add focused tests in `tests/unit/test_installer.py` covering immediate success without sleeping, temporary failures followed by success, exhausted retries, and preservation of the existing-destination overwrite behavior.
3. Add a user-facing bug-fix entry for #20118 to `user_docs/en/changes.md`, as required by NVDA's contribution guide.
4. Run the focused installer tests and the full checks listed in `ship-manifest.md`: unit tests, lint, translation-comment checks, and dependency-license checks.
5. Build and install the changed launcher, then repeat the controlled byte-range-lock reproduction. Verify an unlocked baseline, a temporary lock released during the retry window that now succeeds, and a persistent-lock control that still fails after the bounded attempts.
6. Record the before-and-after observations and check results in `test_evidence.md` and the contribution story. If implementation requires a material change from this plan, update `plan.md` and record the reason before continuing.

No deviations from the accepted plan were required during implementation.

**Toolkit**

- Tool: I used `plan-check` to grade `plan.md` and my draft issue comment against the live issue, reproduction evidence, repository conventions, and planning rubric.
- What it produced: It accepted a bounded retry plan for `installer.tryCopyFile`, four focused unit-test cases, the controlled before-and-after reproduction, and a user-facing change-log entry.
- Difficulties: The first run rejected my draft comment because it referred to reproduction evidence that had not been shared on the issue thread and omitted the limits of my controlled test.
- What changed: I added the environment, byte-range-lock steps, error 33 result, successful controls, and unknowns to the draft comment. I also documented the added delay for other `tryCopyFile` callers and added the required change-log step.

---

## Testing Strategy

I added four focused tests in `tests/unit/test_installer.py`. They verify that an immediate successful copy does not sleep, temporary failures are retried until a copy succeeds, exhausted attempts raise the copy error, and an existing destination still uses the original overwrite fallback without retrying. Before implementation, the two retry tests failed because `tryCopyFile` did not accept the retry parameters. After implementation, all 106 installer tests passed.

I rebuilt and installed the changed launcher and repeated the controlled byte-range-lock test. The unlocked baseline succeeded, and a lock released 750 milliseconds after the settings copy began now succeeded during the retry window. A lock held through all attempts still produced error 33, and the copy succeeded again after the lock was released. The full 1,502-test suite, lint, translation checks, license checks, and `git diff --check` all passed.

**Toolkit**

- Tool: I used NVDA’s unit-test runner and a PowerShell byte-range-lock harness to verify the retry behavior at both the function and installed-application levels.
- What it produced: The tests showed the retry cases failing before implementation and all 106 focused tests passing afterward. The live test showed a temporary lock now succeeding while a persistent lock still failed after bounded attempts.
- Difficulties: The default NVDA log recorded the final error but not the individual `debugWarning` retry messages, and a timed release had to occur after the copy actually began.
- What changed: I used a background job that watched for the previous destination to disappear and released the lock 750 milliseconds later, giving a repeatable test inside the retry window.

---

## Implementation Notes

**2026-10-30**

I created `fix/20118-retry-settings-copy` from current upstream commit `0041ab653`. I first added four focused `tryCopyFile` tests. The two new retry tests failed against the unchanged function with `TypeError`, while the immediate-success and overwrite-fallback controls passed.

I added configurable retry attempts and intervals to `source/installer.py`, preserving the existing overwrite fallback. I added the focused tests to `tests/unit/test_installer.py` and the required user-facing entry to `user_docs/en/changes.md`. The focused tests, full suite, repository checks, launcher build, and controlled installed-build reproduction all passed.

**Toolkit**

- Tool: I used Codex to translate the accepted plan into a test-first patch, review the three-file diff, and guide the controlled installed-build verification.
- What it produced: The work changed `tryCopyFile`, added four regression tests, and documented the user-visible fix in the NVDA change log.
- Difficulties: The retry had to allow temporary source locks to clear without changing the existing-destination overwrite path or hiding permanent failures.
- What changed: I began with failing retry tests, implemented the bounded loop, retained the overwrite fallback, and added a persistent-lock control after the temporary-lock success.

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

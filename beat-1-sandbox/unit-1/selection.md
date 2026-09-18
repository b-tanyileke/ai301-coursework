# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

[Issue #68](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68)

**Verdict output**

## Summary ΓÇö Issue #68: `KeywordSearcher.index()` raises `ZeroDivisionError` on empty corpus

| Check | Grade | Evidence |
|---|---|---|
| Repository is active | pass | Not archived; last push 2026-09-16 (2 days before evaluation), with multiple commits by collaborator Aburke225 that same day |
| Maintainer is responsive | pass | Collaborator Aburke225 (COLLABORATOR association) closed issues #52 and #43 with substantive responses the same day they were filed |
| Scope is bounded | pass | Body names the exact bug (`index([])` ΓåÆ `ZeroDivisionError` in `BM25Okapi`), the two files to touch, the fix requirement (handle empty corpus like `search()` does), and instructs removing the `@pytest.mark.xfail` marker; effort estimated 2ΓÇô4 hours |
| Issue is unclaimed | pass | No assignees, zero comments, and `gh pr list --state all` returns no PRs in the entire repo |
| AI-assisted contribution is allowed | pass | No `CONTRIBUTING.md` or AI policy file anywhere in the repo (root or `.github/`); PR template has no AI-disclosure requirement; silence passes per rubric |

All required checks pass, and the preferred responsiveness check also passes. This is a well-scoped Python bug fix (`rag/retriever/keyword_search.py`) with a `good first issue` + `tier-1` label, which matches your stated preference for a clearly scoped Python issue over HTML/CSS/JS or architectural work.

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68",
  "checks": [
    {"name": "Repository is active", "grade": "pass", "evidence": "last push 2026-09-16T21:50:20Z, not archived, multiple recent commits by Aburke225"},
    {"name": "Maintainer is responsive", "grade": "pass", "evidence": "Aburke225 (COLLABORATOR) replied same-day on issues #52 and #43"},
    {"name": "Scope is bounded", "grade": "pass", "evidence": "Body specifies exact bug, files (keyword_search.py, test_keyword_search.py), required behavior, and removal of xfail marker H-01"},
    {"name": "Issue is unclaimed", "grade": "pass", "evidence": "No assignees, 0 comments, `gh pr list --state all` returns []"},
    {"name": "AI-assisted contribution is allowed", "grade": "pass", "evidence": "No CONTRIBUTING.md or AI policy file found in repo root or .github/; silence passes"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Smoke run with `--limit 3`: `agreement: 3/3 scored items`
2. Complete 20-issue run: `agreement: 19/20 scored items (bar: 18/20: PASS)`

**Issue analysis**

I analyzed `issue-19`. My rubric returned `reject`, while the gold label was `accept`. The harness reported `failed: Scope is bounded`. The issue said, “There are two potential causes which should be fixed,” and then listed three “Additional suggestions,”. My rubric Likely interpreted that as several changes rather than one bounded beginner task.

**Check rationale**

> **Scope is bounded**  
> **Evidence:** Inspect the issue body and comment thread for the requested change, expected behavior, design decisions, dependencies, and earlier attempts.  
> **Pass condition:** The issue requests one bounded code, documentation, or test change whose intended result is sufficiently clear to begin investigating. Fail tracking or umbrella issues, pure support questions, unresolved design discussions, or issues with repeated abandoned attempts that indicate hidden complexity.  
> **Weight:** required

I made this check required because I wanted a relatively simple first contribution with a clear intended result which isn't complex. I included the issue body and discussion as evidence to identify unresolved questions or abandoned attempts which could signal complexity.

**Trade-offs**

My scope check rejected `issue-19`, which the gold label accepted. It may therefore miss some viable issues with multiple causes or implementation strategies. I'm fine with this trade-off for my first contribution because, I wouldn't want to accept work involving several interacting changes or multiprocessing.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[Answer all three:

1. The issue's fit to your interests and to the time available.
2. What the verdict identified correctly, and what you weighed that the rubric could
   not.
3. The anticipated difficulty in claiming it.]

1. This issue fits my interests because it is a clearly scoped Python bug in the RAG subsystem, which is related to my interests in AI and machine-learning. The issue estimates 2–4 hours and provides an existing failing test, so it appears reasonable.

2. The verdict correctly identified that the repository is active, the issue is unclaimed, AI-assisted work is not prohibited, and the expected behavior is sufficiently clear. Beyond the rubric, I weighed my familiarity with Python and RAG systems.

3. I expect the claim itself to be straightforward because the issue is open and currently has no assignee, comments, or linked pull request.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.

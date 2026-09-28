# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

b-tanyileke

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-5858555005

Hi, I’d like to work on the empty-index failure in `KeywordSearcher.index()`. I’ll reproduce the `ZeroDivisionError` from calling `index([])` on the current repository state, record my environment and exact steps, and post the output here before attempting a fix. This is my first contribution to this repository.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-5859226023

I reproduced the empty-index failure on the current `main` code.

### Environment

- OS: Windows x64, build 10.0.26200
- Python: 3.12.4
- PathReview: 0.1.0
- `rank-bm25`: 0.2.2
- pytest: 9.1.1
- Code state: commit `2f4e82f52efbcfcc57d65b3fa5348672163ca088`
- Working tree: clean

### Setup

From a fresh clone of my fork:

```powershell
git checkout 2f4e82f52efbcfcc57d65b3fa5348672163ca088
conda run -n base python -m venv .venv
.\.venv\Scripts\python.exe -m pip install --upgrade pip setuptools wheel
.\.venv\Scripts\python.exe -m pip install -e ".[dev]"
```

The covering test is currently marked `xfail`, so I used `--runxfail` to expose the underlying failure:

```powershell
.\.venv\Scripts\python.exe -m pytest tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index -vv --runxfail --tb=short
```

Relevant output:

```text
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index FAILED

tests\unit\test_keyword_search.py:140: in test_empty_index
    searcher.index([])
rag\retriever\keyword_search.py:25: in index
    self.bm25 = BM25Okapi(tokenized_corpus)
.venv\Lib\site-packages\rank_bm25.py:52: in _initialize
    self.avgdl = num_doc / self.corpus_size
E   ZeroDivisionError: division by zero

1 failed
```

I also reproduced it directly:

```powershell
.\.venv\Scripts\python.exe -c "from rag.retriever.keyword_search import KeywordSearcher; KeywordSearcher().index([])"
```

This produces the same `ZeroDivisionError` in `rank_bm25._initialize()`.

As a control, calling `search()` without first building an index returns an empty list:

```powershell
.\.venv\Scripts\python.exe -c "from rag.retriever.keyword_search import KeywordSearcher; print(KeywordSearcher().search('python'))"
```

```text
2026-09-27 14:16:08 [warning  ] keyword_search_empty_index
[]
```

Expected: `index([])` handles an empty corpus without raising, allowing an empty search to return `[]`.

Actual: `index([])` constructs `BM25Okapi` with an empty tokenized corpus. `rank_bm25` then divides by a corpus size of zero and raises `ZeroDivisionError`.


## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke run with `--limit 3`: 3/3 agreement.
2. First full run: 17/20 agreement; disagreements on `pkg-05`, `pkg-09`, and `pkg-10`.
3. Targeted revision run on `pkg-05`, `pkg-09`, and `pkg-10`, with `pkg-14`, `pkg-17`, and `pkg-18` as canaries: 6/6 agreement.
4. Full run after those revisions: 19/20 agreement, but below the bar because the disclosure category was 0/1; `pkg-20` was incorrectly accepted.
5. Targeted disclosure run on `pkg-07`, `pkg-09`, and `pkg-20`: 3/3 agreement.
6. Repeated targeted disclosure run on the same packages: 3/3 agreement.
7. Final full saved run: 19/20 agreement, every category matched, PASS.

**Package analysis**

I analyzed `pkg-03`. My rubric returned `reject`, while the gold label was `accept`. The package faithfully reproduced the ripgrep line-number bug with the exact input and command, recorded the environment and version difference, and included a control showing that removing `--replace` restores the correct line numbers. The rejection came from `repo-conventions-followed` and `required-ai-disclosure`. The repository policy says maintainer comments must be written by humans in their own words, while my disclosure check tells the evaluator to treat eval packages as AI-assisted. The grader appears to have interpreted that combination as violating the comment policy even though the package’s comment is specific and natural.

**Check rationale**

| required-ai-disclosure | The repository facts' contribution or AI-use policy read against the candidate claim and reproduction comments. | First determine whether the stated disclosure rule applies to issue comments, all AI use, or only pull requests and code contributions. In eval mode, treat the candidate package as AI-assisted work. In live mode, a draft checked or revised with this skill is AI-assisted. Pass when no disclosure requirement applies to these comments, or when the comments include every required disclosure, including the tool and extent of assistance when requested. Fail when an applicable disclosure is required but missing. Do not extend a pull-request-only disclosure rule to issue comments. | required |

I added this dedicated check after disclosure was handled only inside the broader `repo-conventions-followed` check. The broad check rejected `pkg-20` in one full run but accepted it in the next, causing the category floor to fail even with 19/20 agreement. I made the policy scope explicit so the grader distinguishes rules applying to all AI use or issue comments from rules limited to code and pull requests.

**Trade-offs**

The dedicated disclosure check reliably catches `pkg-20`, but its eval-mode assumption contributed to the false rejection of `pkg-03`. I accepted that trade-off because missing `pkg-20` fails the disclosure category floor, while the final rubric still achieved 19/20 overall. I used `pkg-07` as a canary for a required disclosure that was present and `pkg-09` as a canary for a policy limited to code or pull requests; both remained accepted in two targeted runs.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.

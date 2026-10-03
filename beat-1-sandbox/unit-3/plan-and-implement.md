# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

b-tanyileke

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-5965596361

Based on my reproduction at commit
2f4e82f52efbcfcc57d65b3fa5348672163ca088, index([]) passes an
empty tokenized corpus to BM25Okapi, which divides by a corpus size of
zero during initialization. My control showed that search() already
returns [] when no index exists, so I plan to handle the empty case
inside KeywordSearcher.index() before BM25 construction.

The change will store the empty chunk list, reset self.bm25 to
None, and return while leaving the non-empty indexing path unchanged.
In tests/unit/test_keyword_search.py, I will remove the issue #68
xfail marker and add coverage for replacing an existing non-empty
index with an empty one, so stale BM25 state cannot remain.

I will verify the change by rerunning my posted empty-index reproduction,
confirming that a subsequent search returns [], and running the full
keyword-search unit test file. I am keeping search(), tokenization,
BM25 scoring, HybridRetriever, and the rank-bm25 dependency out of
scope.

I also reviewed the classmate PRs currently linked to this issue (#74
and #83). This remains my independent plan based on my own reproduction.

---

## Your branch

**Branch**

fix/68-empty-index

**Evidence**

### Before the fix

At commit `2f4e82f52efbcfcc57d65b3fa5348672163ca088`, I ran the Unit 2 reproduction test:

```powershell
.\.venv\Scripts\python.exe -m pytest tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index -vv --runxfail --tb=short
```

- Output:

tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index FAILED

tests\unit\test_keyword_search.py:140: in test_empty_index
    searcher.index([])
rag\retriever\keyword_search.py:25: in index
    self.bm25 = BM25Okapi(tokenized_corpus)
.venv\Lib\site-packages\rank_bm25.py:52: in _initialize
    self.avgdl = num_doc / self.corpus_size
E   ZeroDivisionError: division by zero

1 failed

### After the fix

```powershell
.\.venv\Scripts\python.exe -X utf8 -m pytest tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index -vv --runxfail --tb=short
```

- Output:

collected 1 item

tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index PASSED [100%]

1 passed in 0.33s

```powershell
.\.venv\Scripts\python.exe -X utf8 -c "from rag.retriever.keyword_search import KeywordSearcher; s = KeywordSearcher(); s.index([]); print(s.search('python'))"
```

- Output

2026-10-03 00:02:04 [warning  ] keyword_search_empty_index
[]

```powershell
.\.venv\Scripts\python.exe -X utf8 -m pytest tests/unit/test_keyword_search.py -vv --tb=short
```

- Output

tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index PASSED
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index_clears_existing_state PASSED

18 passed in 0.30s

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke run with `--limit 3`: 3/3 agreement.
2. First full saved run: 20/20 agreement, with every category fully matched; PASS.

**Package analysis**

I analyzed `pkg-10`. My rubric returned `reject`, and the gold label was
also `reject`. The plan identified `git_status` as the slow component,
but it left the actual solution to implementation time: it proposed
profiling, investigating the Scoop installation, exploring caching, and
optimizing whatever appeared slow. My `executable-by-stranger` check
rejected that because the plan did not select a technical layer,
mechanism, or starting code area. The `test-decisive` check also rejected
“the prompt should feel fast” and “timings should look much better”
because neither statement gives an observable threshold that would
distinguish a successful fix from the reproduced 1.9-second delay.

**Check rationale**

| scope-bounded | The candidate plan's in-scope and out-of-scope commitments, files or code areas, approach, and stated deferrals read against the issue and thread highlights. | The plan describes one coherent, reviewable change and excludes unrelated refactors, migrations, redesigns, cleanup, or adjacent defects. A deliberately scoped-down solution passes when its boundary and deferrals are explicit and it still resolves the behavior the plan claims to fix. | required |

I wrote this check to judge whether the proposed change is a coherent
review unit rather than judging how large or comprehensive the plan
looks. The lecture emphasized that the out-of-scope boundary is what a
reviewer can hold the eventual diff against. I also allowed explicit
scoped-down solutions because deferring a larger redesign can be the
more responsible plan when the remaining change still resolves a useful,
well-defined part of the issue.

**Trade-offs**

The `scope-bounded` check deliberately allows a contributor to defer
part of a larger issue when the deferral is explicit and the remaining
change is complete on its own. The trade-off is that it may accept a
narrower solution than a maintainer ultimately prefers. I accepted that
risk because requiring every plan to solve the broadest version would
reward scope creep. the rubric accepted the intentionally scoped-down
`pkg-09` and `pkg-14`, while it rejected all four scope-creep packages
(`pkg-06`, `pkg-12`, `pkg-15`, and `pkg-19`).

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.

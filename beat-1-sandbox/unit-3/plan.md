# Plan for issue 68 Empty keyword index

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68

Reproduction code state: `2f4e82f52efbcfcc57d65b3fa5348672163ca088`

## Diagnosis

`KeywordSearcher.index()` stores the supplied chunks and then always
constructs `BM25Okapi` from the tokenized corpus. When `chunks` is an
empty list, the tokenized corpus is also empty.

My reproduction reached this path:

```text
tests\unit\test_keyword_search.py:140: in test_empty_index
    searcher.index([])
rag\retriever\keyword_search.py:25: in index
    self.bm25 = BM25Okapi(tokenized_corpus)
.venv\Lib\site-packages\rank_bm25.py:52: in _initialize
    self.avgdl = num_doc / self.corpus_size
E   ZeroDivisionError: division by zero
```

This shows that `index([])` passes an empty corpus into `BM25Okapi`,
whose initialization divides by a corpus size of zero.

The control from my reproduction also showed that the search path
already handles an absent index:

```text
2026-09-27 14:16:08 [warning  ] keyword_search_empty_index
[]
```

Therefore, the failure is not in `search()`. The missing behavior is an
empty-input guard in `index()` before it constructs `BM25Okapi`.

A read-only state check also showed that if a searcher first indexes a
non-empty corpus and then calls `index([])`, the exception occurs after
`self.chunks` has been replaced with `[]` but before `self.bm25` has
been replaced. The object is left with empty chunks and the previous
BM25 index. The empty path should clear both pieces of index state.

## Scope

### In scope

- Handle an empty `chunks` list in `KeywordSearcher.index()` without
  constructing `BM25Okapi`.
- Store the empty chunk list and reset `self.bm25` to `None` so a
  previously populated searcher cannot retain a stale internal index.
- Remove the strict `xfail` marker for issue #68 from the existing
  `test_empty_index` test.
- Add focused coverage for replacing an existing non-empty index with
  an empty one.

### Out of scope

- Changes to `KeywordSearcher.search()` or `_tokenize()`.
- Changes to BM25 scoring or result ordering.
- Changes to `HybridRetriever`.
- Changes to the `rank-bm25` dependency.
- Handling a non-empty chunk list whose text fields tokenize to no
  words. That is a different input and failure boundary from
  `index([])`.
- Unrelated keyword-search cleanup or refactoring.

## Files

- `rag/retriever/keyword_search.py`
- `tests/unit/test_keyword_search.py`

## Approach

1. Keep assigning the supplied list to `self.chunks`, preserving the
   current behavior for both empty and non-empty inputs.
2. Immediately check whether the list is empty.
3. For an empty list, set `self.bm25` to `None` and return before
   tokenization and `BM25Okapi` construction.
4. Leave the existing tokenization, BM25 construction, scoring, and
   successful non-empty indexing path unchanged.
5. Remove the `@pytest.mark.xfail` marker from `test_empty_index`, as
   required by the issue and the repository contribution guide.
6. Add a regression test that indexes a non-empty corpus and then
   indexes an empty list. Verify that subsequent search results are
   empty and that the prior BM25 index is no longer retained.

## Test plan

### Existing issue test

Run the existing test without bypassing or expecting an xfail:

```powershell
.\.venv\Scripts\python.exe -X utf8 -m pytest tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index -vv --tb=short
```

Expected after the fix:

```text
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index PASSED
```

There should be no `ZeroDivisionError`, `XFAIL`, or strict `XPASS`.

### Direct reproduction

Re-run the direct empty-index case and continue into `search()`:

```powershell
.\.venv\Scripts\python.exe -X utf8 -c "from rag.retriever.keyword_search import KeywordSearcher; s = KeywordSearcher(); s.index([]); print(s.search('python'))"
```

Expected after the fix:

- The command finishes without a traceback.
- The existing `keyword_search_empty_index` warning is emitted.
- The final printed value is `[]`.

### Re-index regression

Run the new regression test that first creates a non-empty index and
then replaces it with an empty index:

```powershell
.\.venv\Scripts\python.exe -X utf8 -m pytest tests/unit/test_keyword_search.py -k "empty_index" -vv --tb=short
```

Expected after the fix: both the original empty-index test and the
re-index-to-empty test pass, and the prior BM25 state is not retained.

### Keyword-search regression file

Run the complete keyword-search unit test file:

```powershell
.\.venv\Scripts\python.exe -X utf8 -m pytest tests/unit/test_keyword_search.py -q
```

Expected after the fix: all tests pass with no remaining xfail for
issue #68. Existing non-empty indexing, scoring, ordering, and search
behavior remain unchanged.

## Risks and unknowns

- Resetting `self.bm25` to `None` makes an empty re-index equivalent to
  the uninitialized state that `search()` already handles. A repository
  search found no code outside `KeywordSearcher` that directly reads
  `.bm25`, so this should not break an external state dependency.
- The early return means an empty input does not emit the existing
  `keyword_index_built` success log. The current implementation raises
  before reaching that log, and the issue does not define logging for
  an empty index, so I will not add a new logging behavior as part of
  this fix.
- Non-empty chunks containing only blank text may expose a different
  `rank-bm25` edge case. That input is not established by the issue or
  my posted reproduction and is intentionally deferred.

## Deviations

There were no deviations from the posted plan. The implementation added
the planned empty-input guard, cleared the prior BM25 state, removed the
existing `xfail` marker, and added the planned re-index regression test.
Only the two files identified in the plan were changed.

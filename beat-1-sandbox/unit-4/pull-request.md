# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/121

**Branch**

fix/68-empty-index

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Initial selective run on `pkg-02`, `pkg-03`, `pkg-04`, `pkg-12`, and
   `pkg-20`: 0/0 scored items because all five Claude processes exited
   with errors.
2. Retried selective smoke run on the same five packages: 5/5 agreement.
3. First full run: 15/20 agreement. The disagreements were `pkg-05`,
   `pkg-08`, `pkg-11`, `pkg-16`, and `pkg-19`.
4. Targeted revision run on `pkg-01`, `pkg-04`, `pkg-05`, `pkg-08`,
   `pkg-11`, `pkg-16`, `pkg-17`, and `pkg-19`: 7/8 agreement.
   `pkg-08` was the remaining disagreement.
5. Final full saved run: 18/20 agreement, with every category matched;
   PASS. The remaining disagreements were `pkg-08` and `pkg-16`.

**Package analysis**

I analyzed `pkg-08`. My rubric returned `reject`, while the gold label
was `accept`.

The repository template required the code to be formatted. The candidate
description said `"code formatted"`, but its test evidence only reported:

> Integration test `stash_untracked_only_errors` passes; `go test ./...`
> passes; `go generate ./...` produces no cheatsheet diff.

It did not name a formatter invocation or show a formatter result. My
`required-checks-reported` check therefore treated the formatting claim
as asserted rather than verified. The gold label accepted the
package, so my rubric is more conservative.

**Check rationale**

| required-checks-reported | The plan's regression checks and the repository's required checks read against the candidate test evidence and truthful checklist state. | Every applicable planned or repository-required check has a named invocation and a specific visible outcome. In an eval bundle, a concise result such as a named suite passing with a count, or a named formatter or linter reported clean, is sufficient unless the package contradicts it; do not require line-by-line stdout. In live mode, require the actual captured output requested by the course workflow. A failing or unavailable check may still satisfy evidence reporting when stated honestly with its result and reason, unless the result demonstrates that the change violates a required behavior. A hidden, skipped, vague, or merely asserted check fails. | required |

I revised this check after the first full run rejected several
clear-accept packages for incomplete check reporting. The current
version allows concise named results in eval packages instead of
requiring line-by-line terminal output, while live mode still requires
the captured output requested by the course workflow. The targeted
revision run reached 7/8 agreement, and the final full run reached
18/20.

**Trade-offs**

The `required-checks-reported` check trades some acceptance for
verifiability. It accepts concise results rather than requiring complete
terminal transcripts, but it still rejects `pkg-08` because `"code
formatted"` does not identify a formatter command or result. The gold
label accepted that package, so this choice costs one match.

I accepted that trade-off because the same evidence standard matched all
four `not-tested` packages in the final run while still accepting five
of the seven clear-accept packages.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.

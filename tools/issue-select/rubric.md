# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
| --- | --- | --- | --- |
| Repository is active | In the repo-facts block, inspect the archived flag, latest release, last push, and last 5 default-branch commits. A bot merge of a human pull request counts as human project activity, but a bot-only dependency or generated-content update does not. | The repository is not archived, and it has either a release within 365 days or at least one qualifying human commit or bot merge of a human pull request within 180 days of the capture date. | required |
| Maintainer is responsive | Inspect the maintainer first-response sample in the repo-facts block and owner, member, or collaborator comments in the issue thread. | At least one sampled issue or the current issue received an owner, member, or collaborator response within 30 days, or the current issue was opened by a maintainer. | preferred |
| Scope is bounded | Inspect the issue body and comment thread for the requested change, expected behavior, design decisions, dependencies, and earlier attempts. | The issue requests one bounded code, documentation, or test change whose intended result is sufficiently clear to begin investigating. Fail tracking or umbrella issues, pure support questions, unresolved design discussions, or issues with repeated abandoned attempts that indicate hidden complexity. | required |
| Issue is unclaimed | Inspect the assignees and linked pull requests in the repo-facts block and read the comment thread for active claim statements or mentioned pull requests. | There is no assignee, no open pull request implementing the issue, and no active claim made within 30 days of the capture date. An older claim passes only when there is no open pull request and later evidence shows that the attempt was abandoned or that a maintainer invited new contributors. | required |
| AI-assisted contribution is allowed | Inspect the contribution-policy line in the repo-facts block for rules in CONTRIBUTING.md, AI policy files, contributor documentation, and pull-request templates. | Pass when the policy allows AI assistance, allows it with disclosure or human-review conditions, or states no AI policy. Fail only when the project explicitly prohibits AI-generated or AI-assisted code or documentation. | required |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

Accept an issue only when every required check passes. Reject the issue if any required check fails or is unclear. Preferred checks never change the accept-or-reject verdict; use them only to rank issues that pass every required check.

# Issue

**Title:** "Truncate" XFEL expts/refls based on aggregated statistics

## Description

This PR ports the changes discussed in PR #1001 outside postrefinement into a dedicated `truncate` worker. In contrast to `postrefine` or `filter` workers, the `truncate` worker is strictly designed to act on the entire dataset and utilize global communication instead of refraining from using it. For the time being, it utilizes only the "intensity extrema threshold" mechanism described in PR #1001; however, it is strictly designed to be expanded by additional "truncation" criteria.

For more details on the mechanism behind the rejection mechanism, refer to #1001.

Bugfixes and code clean-up introduced to the `postrefine` worker are not moved here and have been made into a separate PR #1002 instead.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8d1a1a752b17`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
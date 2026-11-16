# Issue

**Title:** Fix/432 by making sure modifications are connected components

## Description

`canonicalize_modificaitons.identify_ptms` assumes modifications are connected components. If this assumption is violated, in combination with the `-modify` flag, this causes hard to debug user facing errors ( for more background).

This PR resolves that by
1) Log an error for any modification that is not a connected component.
2) Modify the maxwarn mechanism to count all messages >= warning, meaning an error will cause no files to be written. Note that the `-maxwarn` flag cannot override errors, only warnings.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `77637bffbc36`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
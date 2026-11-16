# Issue

**Title:** Fix 408

## Description

keep track of max-node instead of searching it each time merge_molecule is called. This should fix issue #408 and as far as I can tell not break anything else. The only difference with the current implementation would be that if nodes are removed the max count is not reset, which is OK. Still all ids are unique. 

Interesting point of order here: If in the current branch a node is removed in the middle of the molecule graph and not at the end it will probably break, because the same resid is assigned twice. This does not happen with this fix.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c437876bd494`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
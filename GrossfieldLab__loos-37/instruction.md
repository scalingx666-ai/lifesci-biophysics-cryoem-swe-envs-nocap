# Issue

**Title:** Gitignore upgrade

## Description

This uses the fact that new executables should reside within source-containing directories to ignore the contents of those directories, minus a few exception patterns. Leaves 'already tracked' files that would be excluded without a `git add -f` already tracked (for example, the contents of `Packages/OptimalMembraneGenerator/example` would be ignored without a specific add, but because those files are currently tracked any adjustments to them will be tracked as well. If someone were to rename one not using `git mv` or were to add a new file to that directory, it would be ignored. This actually seems fine to me, because I think that in general a user probably doesn't want to accidentally add *.inp files that are floating around the loos directory tree, but this could be changed too if needed. 

Also, builds!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `33c8f806b72b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
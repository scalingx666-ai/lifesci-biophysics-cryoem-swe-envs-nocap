# Issue

**Title:** NAMD parsing: Raise error on truncated but not restarted window

## Description

It's probably a good idea to detect when there are truncated windows that aren't restarted (issue #145). Previously those energies would be stuck on to the following window - not ideal. This patch raises errors for this situation, and includes tests. In addition, fepout files are now consumed in order of their filename, not including the directory name (as this can vary due to e.g. tests that modify the test set on the fly).

We could consider supporting the use case of end-of-window being implied by end of file, but that will likely require significant changes to the parser logic, and I'll probably do it at some point.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b611c0b654e3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
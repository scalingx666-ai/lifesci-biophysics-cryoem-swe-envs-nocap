# Issue

**Title:** Close Matplotlib figures after writing

## Description

* Been noticing this warning in tests a lot recently and have seen OOMs from running many RBFE calculations in the same process. Not sure it is the plotting, but a super simple change so figured I would resolve this.
* Dependent on the Matplotlib backend. Doesn't fail in CI because there is no GUI, locally this does fail. Makes this a pretty insignificant change, would still advocate for it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `39015e35985b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
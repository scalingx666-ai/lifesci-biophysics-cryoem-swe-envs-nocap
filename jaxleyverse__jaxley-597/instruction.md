# Issue

**Title:** Fixes for `groups` after `.set_ncomp()`

## Description

In #560, we identified that groups do not get updated automatically when `set_ncomp()` is run. This PR fixes that by making `groups` a first-class citizen: Group identities are listed as a boolean column in `.nodes` now. With this, we need no further special treatment of `set_ncomp()`. In addition, we can get rid of special treatment of groups in the `graph` swc reader.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b9eb99bebba6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
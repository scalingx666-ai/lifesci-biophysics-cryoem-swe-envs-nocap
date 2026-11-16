# Issue

**Title:** xfel: removed missed unused imports

## Description

There was a bug in the clutter code where words in doc strings were treated as code. The fix revealed more imports that are unused.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7a69196b9923`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
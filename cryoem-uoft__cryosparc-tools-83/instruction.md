# Issue

**Title:** Refactor: Dataset interface updates for future improved job I/O

## Description

- New `rename` param for filter prefix to allow pulling out a renamed subset of fields in one shot
- New `is_equivalent` method to check whether datasets have identical fields that may be out of order
- Require that `copy` param is specified as kwarg
- Removed load_cached method

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `16fbb6479dd9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
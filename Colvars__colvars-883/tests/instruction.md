# Issue

**Title:** fix: fix the typo in using accumulate_cpu_force_kernel

## Description

This is a typo in calling `accumulate_cpu_force_kernel`. The code path is never tested since atom groups seldom have multiple parents.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `66df73a39242`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
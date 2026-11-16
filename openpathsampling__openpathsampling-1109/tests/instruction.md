# Issue

**Title:** Prevent double monkey-patching experimental storage

## Description

It was possible to double monkey-patch for the experimental storage. This caused many headaches downstream (e.g., OPS CLI) because that meant that something downstream had to track state of whether OPS had been monkey-patched. It also might be the problem behind #1108. This PR makes it so that monkey-patching is idempotent.

Marked WIP because it still needs tests (which will be slightly a pain), but this is the implementation I'm planning on.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `91318915aa0c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
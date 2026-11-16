# Issue

**Title:** Insanity

## Description

This is a fairly extensive reorganization of the sanity_check module.

Now, similar actions are handled by a single function, instead of patching
it whenever needed.
It eliminates a few functions by incorporating them into other, greatly helping
readability
Also, some functions are renamed and a bit of documentation is added.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `33305ef7ef38`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
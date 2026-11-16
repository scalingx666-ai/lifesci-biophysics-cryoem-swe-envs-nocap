# Issue

**Title:** Deferring

## Description

This PR changes file_writer.open to file_writer.deferred_open and adds defer_writing=TRUE as boolean keyword argument to writing functions, so as to allow both direct as well as deferred writing (e.g. in write_pdb). Overwriting the function 'open' with file_writer.DeferredWriter().open was never a good idea.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4261733ffa18`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
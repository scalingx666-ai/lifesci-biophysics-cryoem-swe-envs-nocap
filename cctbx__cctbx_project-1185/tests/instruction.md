# Issue

**Title:** Issue 1173 rebase

## Description

Moved hydrogen-removal out of Optimizers.Optimizer() and into the caller, which is reduce2 and other programs.

@vbchen you will need to add code to your use of this that matches what is done in mmtbx/hydrogens/__init__.py before you reinterpret the model in your branch once this is merged.

It also returns a list of flips that were made, whether they were due to locking down a configuration or optimizing a Mover; this is to help with issue 1173.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a287119d9cc7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
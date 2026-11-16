# Issue

**Title:** Fix trying to detach a float number in LNNP

## Description

If the user trains with only energy or only forces in some situations the LNNP class will try to call detach on a float where it expects a tensor.
I changed the loss computation so it initializes the losses on both energy and force as a torch.tensor.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2cc539561a8f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Support for float64

## Description

I added a new parameter, dtype, to control whether TorchMD_Net uses float or double.
The value can be passed along the rest to create_model, but it defaults to float if not present. Can be either a string or a torch.dtype.

I also added some comments here and there.

To showcase I added a test that checks the correctness of gradients for all models using torch.autograd.gradcheck.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `070a3b30e6e1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
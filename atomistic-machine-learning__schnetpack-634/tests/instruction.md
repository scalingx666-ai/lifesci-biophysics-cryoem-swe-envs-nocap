# Issue

**Title:** restructure embeddings to avoid issues with torch jit

## Description

I restructured the embeddings to avoid problems with torch jit.

Nuclear and electronic embeddings are separated, because electronic embeddings currently require `(embedding, inputs)` as arguments in the forward pass and nuclear embeddings only require `atomic_numbers` in the forward pass.
With this, we can further use `torch.nn.Embedding` as a default value and electronic embeddings can be added as `nn.ModuleList`.

This is still a draft and not tested!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `337fa3f80372`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
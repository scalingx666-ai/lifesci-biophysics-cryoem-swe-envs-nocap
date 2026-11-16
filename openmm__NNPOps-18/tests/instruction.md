# Issue

**Title:** CFConv layer for SchNet

## Description

I've written the CPU implementation so far.  The calculation matches the behavior of SchNetPack, which is different from what's described in the paper.  It's less flexible than SchNetPack though.  For example, it always uses a cutoff, and always uses a cosine cutoff function, where SchNetPack lets you choose an arbitrary cutoff function.

The way I've implemented the derivatives is less than ideal.  The standard method for doing backpropagation assumes you've saved intermediate quantities during the forward pass.  But I don't want to save any per-interaction quantities, so instead I have to repeat nearly all the calculations from the forward pass in the backward pass.  This slows it down a bit, but I think it will still end up being much faster on the GPU than if we save and reload per-interaction data.  Once the CUDA implementation is written we can investigate that assumption and see if it's true.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f9716fda2bad`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
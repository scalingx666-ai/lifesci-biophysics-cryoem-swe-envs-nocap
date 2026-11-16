# Issue

**Title:** Fix neighbor backward

## Description

Trying to integrate OptimizedDistance into the equivariant transformer (ET) I realized my module did not supported double backwards (DB), which is required in ET when training with forces.

This PR adds support for double gradient to OptimizedDistance, and adds some tests for it using [torch.autograd.gradgradcheck](https://pytorch.org/docs/stable/generated/torch.autograd.gradgradcheck.html).

After some research I learned about [how to support DB in a custom extension](https://pytorch.org/tutorials/intermediate/custom_function_double_backward_tutorial.html#saving-intermediate-results). I rewrote the backwards pass in full pytorch to take advantage of autograd for the second gradient. Then I discovered Python , but it turns out[ it is compatible if you do it in C++](https://pytorch.org/tutorials/advanced/dispatcher.html#adding-autograd-support). So I translated the full pytorch backwards to C++. Quite the journey -.-

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e20876ff79fe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
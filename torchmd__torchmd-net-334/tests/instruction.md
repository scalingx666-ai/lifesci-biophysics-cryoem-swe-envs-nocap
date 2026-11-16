# Issue

**Title:** Add GLU, Swish, Mish and SwiGLU

## Description

- [x] Added Swish and Mish as possible activation functions.
- [x] Added a Gated Linear Unit (GLU) module ( eq. 1 here https://arxiv.org/pdf/1612.08083 ), but the activation function is a parameter.
- [x] Added the SwiGLU activation function https://arxiv.org/pdf/2002.05202

SwiGLU has learnable parameters and requires knowing the input/output shapes at creation, unlike the rest of the activation functions (e.g. SiLU). So, it won't be able to be selected as an activation function in a yaml file right now.

I added it as a tool for NNP development.

Mish and Swish can be selected via a yaml file.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6dea4b61e24d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
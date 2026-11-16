# Issue

**Title:** Split MC-barostat implementation

## Description

This PR splits the implementation of the isotropic, semiisotropic and anisotropic Monte-Carlo-Barostat into separate functions. This should improve the compatibility with Enzyme AD as noted in .

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9451266cdabf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
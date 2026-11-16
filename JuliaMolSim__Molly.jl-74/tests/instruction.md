# Issue

**Title:** add FENE bond and cosine angle.

## Description

Hi. Here I provided a small piece of code to add FENE bond (following [Kremer, Grest J. Chem. Phys. 92, 5057 (1990)](https://doi.org/10.1063/1.458541)) and the cosine angle potential to it. These are very common interactions for modeling chromatin or many other polymeric systems. I tried to follow the code-style used in Molly and also provided relevant equations in the docs. Please let me know if I missed something. Hope this small piece of code is useful for others.

New files:
* `src/interactions/fene_bond.jl`
* `src/interactions/cosine_angle.jl`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3b5d2b1f4808`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
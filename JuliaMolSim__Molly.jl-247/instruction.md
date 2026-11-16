# Issue

**Title:** Conducting boundary conditions for CoulombReaction field

## Description

I found it useful to have the $\varepsilon_{rf} \rightarrow \infty$ approximation available in the coulomb reaction field interaction. It looks like it was done with the special flag once, but in the current state the expression was multiplied by zero. I've also added the potential formula to the docstring, since it's there for most other interactions.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `aac62f083020`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Skip gradients of atoms outside cutoff

## Description

This addresses #77 by detaching gradients from atoms that are outside the cutoff radius of any other atom and are therefore unable to interact inside the model. Previously this caused gradients of the force loss to become NaN, now these atoms are ignored and a warning is raised.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `31d5cb80f98d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Log a helpful message when an element is not present in the ATOM_MASS dict

## Description

… and then use a relatively high mass since it's probably a metal.

Side-effect of #570. I currently set the loglevel to INFO, since I don't think it usually results in a faulty structure or topology.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `11ae07ccdc3c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** fix: fix the gradients of the eigenvector CVC

## Description

The fit gradients are very significant and cannot be ignored!

The test data of `000_eigenvector_harmonic-fixed` needs to be regenerated as well.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `aa63575609b9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Templates can match whole molecules

## Description

Implements #5178.  This allows a single template to match an entire molecule consisting of multiple residues.  It will be necessary for supporting the OpenFF protein force field.

Still to do:

- [x] `addExtraParticles()` should work with molecule templates
- [ ] Decide what to do when some but not all of the residues in a molecule are matched individually
- [x] Documentation

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e688b86e2047`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
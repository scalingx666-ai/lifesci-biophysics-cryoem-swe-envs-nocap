# Issue

**Title:** various fixes due to upstream updates

## Description

Running CI after a while uncovered failures due to updated upstream

- [ ] missing Python 2.7 support in newest numpy
- [ ] DataFrame ` AttributeError: 'DataFrame' object has no attribute 'convert_objects'`
- [ ] ...

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `79e5594fd2af`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
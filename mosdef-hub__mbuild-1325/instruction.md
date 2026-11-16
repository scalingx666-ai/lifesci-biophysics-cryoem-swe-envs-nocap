# Issue

**Title:** add namers.py with BeadNamer class

## Description

### PR Summary:

This provides a data structure for more fine-grained control over bead naming in the various path generation methods of `build.py`.  Similar to `Termination` and `Bias`, the `BeadNamer` class exists outside of the core path generation logic. This makes it easy to modify, and create custom instances of `BeadNamer` without having to change the underlying path code.

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c4f194ca55b6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
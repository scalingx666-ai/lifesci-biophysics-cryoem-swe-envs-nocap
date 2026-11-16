# Issue

**Title:** Account for PBCs in TargetCorodinate bias and WithinCoordinate terminator.

## Description

### PR Summary:

Neither the `TargetCoordinate` bias or the `WithinCoordinate` terminator correctly accounted for periodic boundary conditions, even though the method they both use (`target_sq_distances`) accepts both PBC bools and box lengths. So, this only requires grabbing this info from `.state` and passing them in to `target_sq_distances`.

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b30dbeea2389`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
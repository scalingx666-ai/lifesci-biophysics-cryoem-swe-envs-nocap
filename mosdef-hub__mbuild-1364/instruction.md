# Issue

**Title:** Make initial point an option for all closed-form Path builders

## Description

### PR Summary:
Being able to specify the coordinate of the first bead for all closed-form paths (straight line, lamellar, zigzag, cyclic, helix, etc..) would be very useful for building up systems that contain multiple paths as it makes it easier to avoid overlaps. This will also make it easier to utilize the add method for paths `pathC = pathA + pathB`. This was already part of `lamellar` and it is straight forward to add this for the others.  It defaults to None to keep the original behavior (straight line starts at 0,0,0, but cyclic starts at -r, 0, 0). 

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `233a9ac1db3a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
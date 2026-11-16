# Issue

**Title:** Add python 3.14 to testing, support OpenMM>=8.4

## Description

### PR Summary:

This drops python 3.10 from testing, and adds python 3.14. It also contains a small bug fix from breaking changes in OpenMM>=8.4. The numpy max pin was also bumped up. Once this is merged,  should pass. I also dropped the windows specific yml file, I don't think we need it any more (or should be used) as we are testing on WSL.

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0383f7d50ed8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
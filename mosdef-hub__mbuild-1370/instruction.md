# Issue

**Title:** Add crystal treatment for orthographic lamellar structures. Create a …

## Description

### PR Summary: 
This code looks to adjust the lamellar builder to handle different orthorhombic configurations of lamellar packings.  This changes the arguments to take a,b,c cell parameters, and number of repeat units. Finally, you can also specify the crystal symmetry using a string argument. The previous behavior is still the default as "primitive" crystal. This is a major API change, so could also just make a separate module in path to handle crystal lamellar, and extract this code there, leaving the old behavior also available in the more basic lamellar structure builder.

### PR Checklist
------------
 - [x] Includes appropriate unit test(s)
 - [x] Appropriate docstring(s) are added/updated
 - [x] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7bced5c469ff`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
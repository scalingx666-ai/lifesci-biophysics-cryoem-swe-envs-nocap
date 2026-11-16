# Issue

**Title:** Bug fix termination condition checking and better cuboid box sampling

## Description

### PR Summary: 
Add tests for handling failing hsrw and pruning excess sites, as well as properly printing out termination summaries. Add overlaps check for cuboid sampling

### PR Checklist
------------
 - [x] Includes appropriate unit test(s)
 - [x] Appropriate docstring(s) are added/updated
 - [x] Code is (approximately) PEP8 compliant
 - [x] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b49a9e440cc2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Fix/ffinput multiple links

## Description

Fix to issue #243  and some light linting. There are still quite some unused variables and overwritten functions and stuff pylint complains about. But that would require linting all parsers starting with the parser_utilities and then all sub-classes thereof. 

@pckroon Shall I make a separate issue?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `019fb3130e46`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
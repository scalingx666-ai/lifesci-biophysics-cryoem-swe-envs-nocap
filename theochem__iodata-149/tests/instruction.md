# Issue

**Title:** Add `load_one` for Q-Chem log file

## Description

Implementation includes
- [x] Lower level parser for Q-Chem log file, 509ba22
- [x] Higher level `load_one` function for Q-Chem log file, 33d2a25
- [x] Tests for Lower level parser and higher level `load_one` function, f8a791a and 9622008
- [x] Molecular orbital object for Q-Chem return
- [x] Fix pycodestyle errors 7e522b3
- [x] Add tests for molecular object opbject in 4132217

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b36513d162f9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
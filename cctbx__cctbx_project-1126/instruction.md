# Issue

**Title:** Version 4.13 of probe2 reports the location of a clash in JSON output…

## Description

… in condensed mode. New version of clashscore2 asks for and parses the JSON output from probe2 so that it can handle residues with more than 3-character names

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `33f5564cb1c9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Numpy 2.5 compatibility with eigen-calcs

## Description

reverts `186d6cd` and #2168
 

Works on 2.4.6: https://github.com/jeremyleung521/mdtraj/actions/runs/35644153179

Works on 2.5.3: https://github.com/jeremyleung521/mdtraj/actions/runs/35645304643

Tagging #2186 for tracking.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0f0b1a0bdb6d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
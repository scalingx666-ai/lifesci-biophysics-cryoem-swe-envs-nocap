# Issue

**Title:** added two new neighbor lists

## Description

I added the pymatgen nbh list and the vesin nbh list as proposed in #756 .
The nbh lists are tested using pytest but the nbh list speed is not benchmarked yet.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1dc71d55a04b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
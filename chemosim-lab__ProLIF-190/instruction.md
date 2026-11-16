# Issue

**Title:** New acceptor SMARTS for halogen bond

## Description

New default pattern for acceptor in `XBAcceptot/XBDonor` to recognize also carbonyles (and other species with double or aromatic bonds), as mentioned in #189 , below is the compassion between old and new pattern 

![obraz](https://github.com/chemosim-lab/ProLIF/assets/41703271/a7d48b9f-154b-48d3-9a78-d887f31c1eac)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `095798d7b747`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
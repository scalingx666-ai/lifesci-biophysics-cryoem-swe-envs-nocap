# Issue

**Title:** implement updated ZBL in tensornet2

## Description

Implements ZBL short range repulsion in the tensornet2 model. Needed for the latest AceFF-2-OMOL model.

https://huggingface.co/Acellera/AceFF-2-OMOL
https://arxiv.org/abs/2601.00581v3

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `833c07708e7d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
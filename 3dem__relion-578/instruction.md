# Issue

**Title:** move init_pdf_orientation on GPU

## Description

by this commit, we eliminate one data transfer and move calculation of init_pdf_orientation on GPU

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `245c441d2112`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Use default RHF for closed-shell singlets in MOPAC input

## Description

The MOPAC input header unconditionally appended "<multiplicity> UHF" (guard was removed in commit 88321106b19340e110d837e9df88fc584052617f), so a singlet was written as "singlet UHF".

Emit no multiplicity/UHF keyword for multiplicity==1, so the default RHF is used; higher multiplicities are unchanged.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5dcfaa4e1d76`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
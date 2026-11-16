# Issue

**Title:** Fix the hanging test on Mac OSX python 3.12+

## Description

Cannot determine the cause so I will just disable it, it seems to be related to the cleanup of memmaps but since we are probably going to outphase them in favor of :hugs: datasets we don't need it

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eab71babf7a2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
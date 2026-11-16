# Issue

**Title:** Mol2 format implementation

## Description

Implementation of mol2 format. 

Some things I don't like so far is that the first key of the atcharge dictionary is always written out, but I do not know how to tell dump_one function which key (charge type) to write out without changing the api.

Edit by @tovrstra: This adresses parts of #37 and #78.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `060ad724dea0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
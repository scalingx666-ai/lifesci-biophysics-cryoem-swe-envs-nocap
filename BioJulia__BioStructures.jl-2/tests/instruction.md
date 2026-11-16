# Issue

**Title:** Make downloadpdb return the filename again

## Description

A while ago downloadpdb returned the path to the downloaded file, which allowed for this:

`prot = read(downloadpdb("1crn"), PDB)`

PR #483 broke this behavior, so here is a simple fix :) 

I also added a test for it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3c302833b4a6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
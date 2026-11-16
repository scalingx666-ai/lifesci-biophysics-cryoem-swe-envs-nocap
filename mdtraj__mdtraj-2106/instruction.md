# Issue

**Title:** move all rcsb url downloads to https

## Description

A few current tests directly downloads pdb from RCSB PDB using HTTP. I think their servers are (probably) refusing non-SSL connections as of yesterday, leading to timeouts (and test failures). Updating the urls to use `https` instead of `http`.

Example of timeout:
https://github.com/mdtraj/mdtraj/actions/runs/19522543080/job/55888739606

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9ca89d82caf6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
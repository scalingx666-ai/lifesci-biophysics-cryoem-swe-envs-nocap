# Issue

**Title:** Python3.14 testing and building

## Description

Thought I was in my own fork but whatever.

Our testing is currently bottlenecked by the lack of a py3.14 compatible release of openmm. I dropped `openmm` here for the time being and things are working fine (i.e. tests run and all pass).

I'll update the PR after Python 3.14 has officially released next Tuesday, then we can merge this in, along with 

EDIT: `cibuildwheel` works fine
https://github.com/jeremyleung521/mdtraj/actions/runs/18227642437/job/51902948368

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3c2cfd593b50`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
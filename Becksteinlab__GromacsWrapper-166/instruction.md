# Issue

**Title:** Support Slurm in gromacs.qsub

## Description

Current GromacsWrapper cannot edit slurm job script correctly due to the contradiction between slurm setting options and Sun Gridengine. 

Changes made in this Pull Request:
 - Add slurm as one option in qsub.


PR Checklist
------------
 - [ ] Tests?
 - [ ] Docs?
 - [ ] CHANGELOG updated?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d96c34f5d7d8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
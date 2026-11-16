# Issue

**Title:** Simplify the clang-tidy check

## Description

This PR uses a standalone shell script to run clang-tidy over all Colvars source files, excluding the Lepton source files. This PR also avoids pulling the apptainer container.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b7b763dbc996`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
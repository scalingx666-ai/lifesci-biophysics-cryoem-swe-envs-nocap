# Issue

**Title:** Replace `setup.py` with `pyproject.toml`, update CI workflows and dev environment.

## Description

### PR Summary:

This PR does the same as  and  to use a `pyproject.toml` file instead of `setup.py` and to update the dev environment and CI workflow.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a69926cbc9fe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
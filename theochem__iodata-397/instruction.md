# Issue

**Title:** Update and clean up GH workflow files and pyproject.toml

## Description

This PR includes some cleanups in the GH workflow files and `pyproject.toml`. (The original goal was just to make PR and review smoother, but I cleaned a little more while I was at it.)

- Remove deepsource step.
- Use recommended license spec in `pyproject.toml`.
- Make workflows conditional on relevant file changes.
- Move oldest version of dependencies to `.github/requirements-old.txt`.
- Minor cleanups.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8d567aaa06b1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** replace setup.py with pyproject.toml

## Description

edit: ready to be reviewed, I made a minor modification

It is recommended to move from `setup.py` to `pyproject.toml`: https://peps.python.org/pep-0621/

This PR has the additional advantage that one can now test the version of schnetpack within the code like

```
import schnetpack
print(schnetpack.__version__)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7aee2e221c04`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Remove mypy constraint, update ignores

## Description

- [x] Remove `mypy =1.8` pin
- [x] Remove some un-used package-level ignores
- [x] Add some new stub packages from `typeshed`

```shell
$ python --version && rm .mypy_cache && mypy -p "openff.toolkit"
Python 3.11.9
Success: no issues found in 29 source files
```
🤞

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `271da911f487`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
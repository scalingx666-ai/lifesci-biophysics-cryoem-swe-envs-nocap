# Issue

**Title:** Fix CI

## Description

- [x] Require qcportal >=70
- [x] Move mypy to conda deps in env yamls (when they were pip deps they'd be broken by some common deps that got pulled in by nagl, since when the `nagl=false` builds uninstalled nagl the common deps would be removed too)
- [ ] ~Update to `python-constraint2` per  This didn't work, see 
- [x] Remove `blacken-docs` from pre-commit action since it was fighting with ruff.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `278cc81140a2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
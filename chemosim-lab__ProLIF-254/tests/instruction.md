# Issue

**Title:** feat: enable type-checking with mypy and poe pipeline

## Description

## Towards issue #249 (subtask of #247)

Completed subtasks are ticked below:

- [x] Add `py.typed` file
- [x] Add `mypy` to dev dependencies
- [x] Configure `mypy`
- [x] Add local `poe` pipelines
- [ ] Add workflow for GitHub Actions (pending)

## **Dependency Version Question**
I have set the required `mypy` version in dev dependencies to `>=1.0.0`, while the latest version is `1.15.0`.  
Should I update it to a more recent version, or is `>=1.0.0` fine?

Additionally, I have added the `poe` pipeline for the `prolif` and `tests` directories, as observed in issue #248, is that fine?

## **References for `mypy.ini` configuration:**
1. [Mypy Configuration File](https://mypy.readthedocs.io/en/stable/config_file.html#config-file)
2. [Using Mypy with an Existing Codebase](https://mypy.readthedocs.io/en/stable/existing_code.html#using-mypy-with-an-existing-codebase)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2afcfdc88a65`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
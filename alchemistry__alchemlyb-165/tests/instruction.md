# Issue

**Title:** prepare 0.5.1 release

## Description

- for #164 
- CHANGES fixes
- miscellaneous last minute fixes
  - visualisation.df_state.plot_dF_state() now correctly raises ValueError instead of NameError when incorrect orientation keyword argument is supplied
  - close figures in vsualisation tests to avoid warnings for too many open figures
  - update API docs with workflow mission statement #160

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5f61841b3ffc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
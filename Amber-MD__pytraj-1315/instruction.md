# Issue

**Title:** change makestructure reference syntax

## Description

```bash
pt.make_structure(traj, command='ref:1-13:1-13', ref=ref)
```

Excluding reference name since we explicitly passing the `ref`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `40fe63bd503b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
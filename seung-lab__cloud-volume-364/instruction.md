# Issue

**Title:** feat: add agglomerate mode for graphene

## Description

@supersergiy something like this?

```
cv = CloudVolume('graphene://...', agglomerate=True)
img = cv[...]
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ab2a46cc41fb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
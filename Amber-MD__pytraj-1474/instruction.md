# Issue

**Title:** accept negative indices

## Description

```
traj = pt.load(fn, top=tn, frame_indices=[-1])
```

close #1465

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `95546a1e557d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
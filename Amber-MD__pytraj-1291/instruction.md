# Issue

**Title:** ADD support write_data (xmgrace, gnu, ...)

## Description

only support 1D array for now. Still having this #1280 issue.

``` python
rmsd_data = pt.rmsd(traj, ref=0)
pt.io.write_data('my.agr', rmsd_data)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `acae8a28a4fb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
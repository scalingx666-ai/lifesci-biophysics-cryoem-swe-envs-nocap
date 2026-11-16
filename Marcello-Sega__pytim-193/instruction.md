# Issue

**Title:** introduced option to avoid automatic interface determination

## Description

passing the 'autoassign=False' option to the various PYTIM class implementations disables the automatic assignment when switching frame.

Usage example:
```
inter = pytim.GITIM(u,group=u.select_atoms('name OW'), autoassign=False)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4950bb0d0339`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** configure codecov

## Description

- fix #287 
- split project status into package and tests
- split patch status, too

Note: check codecov.yaml with
```
curl --data-binary @codecov.yml https://codecov.io/validate
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2815e109a4ef`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
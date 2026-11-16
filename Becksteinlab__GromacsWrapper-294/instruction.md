# Issue

**Title:** testing codecov

## Description

Test different versions of `codecov.yml` to see if anything solves #287 

Validate with
```
curl --data-binary @codecov.yml https://codecov.io/validate
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e7fd8c3c8341`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
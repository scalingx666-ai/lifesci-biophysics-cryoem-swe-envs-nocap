# Issue

**Title:** Set up versioneer

## Description

tag 

`versioningit` doesn't play nicely with what we have in `setup.py` so I went with `versioneer`

This is mostly ~automated,

```shell
$ pip install versioneer
$ versioneer install --no-vendor
```

and then fiddle with the remainder

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ccd0c874361e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
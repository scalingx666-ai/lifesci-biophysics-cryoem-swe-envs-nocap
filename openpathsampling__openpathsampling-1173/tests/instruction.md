# Issue

**Title:** Use `autorelease check` instead of custom script

## Description

We recently bumped up to setuptools 80 in our default builds. This is causing problems because of major changes in [how `setuptools` treats the `develop` command](https://setuptools.pypa.io/en/stable/history.html#v80-0-0).

In any case, instead of having a custom script that uses autorelease, it is better to use the `autorelease check` command, available since autorelease 0.3.0 (2021). That's what this PR will do.

- [x] See if using `autorelease check` fixes (otherwise, fix might be needed upstream in autorelease)
- [x] Remove old `devtools/autorelease_check.py`
- [x] Update autorelease for publishing, too

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b1e93681595e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
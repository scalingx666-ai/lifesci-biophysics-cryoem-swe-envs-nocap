# Issue

**Title:** Pip package 2

## Description

Here is a new PR for making the repo pip installable. I tried to not rename any folders but that interferes with the automatic discovery of setuptools and I got tired of trying to make it rename the module from `pytorch` to `NNPOps`.
https://setuptools.pypa.io/en/latest/userguide/package_discovery.html#automatic-discovery

I tend to go for flat-layout since it's easily importable from a REPL (as in my last PR)
https://setuptools.pypa.io/en/latest/userguide/package_discovery.html#flat-layout

But I ended up going with the src-layout in this PR since it is a bit closer to the layout here
https://setuptools.pypa.io/en/latest/userguide/package_discovery.html#src-layout

But this required that I rename the pytorch folder to NNPOps which is the only major change here.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6c1fcf852b3f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
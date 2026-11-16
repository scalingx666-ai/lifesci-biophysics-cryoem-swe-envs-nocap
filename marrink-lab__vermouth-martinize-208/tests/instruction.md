# Issue

**Title:** Fix networkx intersphinx reference

## Description

This fixes the Travis build error.
https://travis-ci.org/marrink-lab/vermouth-martinize/jobs/586294516#L420

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `470e01003036`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
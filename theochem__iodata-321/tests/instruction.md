# Issue

**Title:** Fix documentation build issues

## Description

Our documentation build on read-the-docs (RTD) is failing. This should fix it, hopefully.

Because this can only be fully tested by merging the PR and monitoring the doc build in RTD, I will merge this soon.

Ideally, we get rid of RTD for the following reasons:

- RTD supports only one admin account per project, i.e. bus-factor 1 by construction, unless one shares passwords, which is a bad idea anyway.
- Testing the RTD build is only possible after merging.

As an alternative, we can build the documentation in a GitHub Action and host it on gh-pages instead.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6435bc3099d6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
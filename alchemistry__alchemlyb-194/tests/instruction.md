# Issue

**Title:** Set a strict python minimum version for install, add automated pypi deployment

## Description

Work done in this PR:
  - two small changes to setup.py and setup.cfg to enforce python>=3.7 and prevent universal wheels (which are meant to also work with py2) from being built.
  - Added automatic deployment to pypi on release, as per: https://github.com/MDAnalysis/GridDataFormats/blob/master/.github/workflows/deploy.yaml (I'm happy saying that bit of code is under BSD-3 if that works with you @orbeckst)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0d701d18e228`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
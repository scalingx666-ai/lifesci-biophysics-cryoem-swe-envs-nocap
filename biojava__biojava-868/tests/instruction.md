# Issue

**Title:** Minimal read support for files with 'branched' entities

## Description

The [carbohydrate remediation project](https://www.mail-archive.com/ccp4bb@jiscmail.ac.uk/msg48559.html) introduces a new entity type: "branched". The type exists in the mmCIF dictionary for quite some time, but from July 2020 it will be used by many files with carbohydrates.

This pull request simply makes sure that the files can be parsed without breaking any existing functionality. It treats the branched chains as non-polymeric chains.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `18f57de72a27`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
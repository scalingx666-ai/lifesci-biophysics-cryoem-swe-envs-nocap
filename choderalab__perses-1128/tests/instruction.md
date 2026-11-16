# Issue

**Title:** Support openff-toolkit 0.11

## Description

## Description

This PR addresses #1127.

**THIS IS A WORK IN PROGRESS**

It's not yet clear whether we will remain compatible with older openff-toolkit versions, but we will attempt to if possible.

## Motivation and context

The openff-toolkit 0.11 included API-breaking changes, such as 

## How has this been tested?

- [ ] Implement tests

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
* Updated to support openff-toolkit 0.11, which included API-breaking changes.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4b3facc4c459`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
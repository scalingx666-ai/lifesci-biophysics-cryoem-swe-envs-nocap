# Issue

**Title:** Avoid changing global context cache behavior on modules imports

## Description

## Description

We are now avoiding changing the global context cache behavior when importing/using modules.

## Motivation and context



## How has this been tested?

Tested manually with snippet found in #968 

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Global context cache behavior is now unchanged when importing modules.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `59c77fbef459`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
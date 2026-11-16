# Issue

**Title:** Examples run phases in expected order

## Description

## Description

Changes the order how phases are run in the examples and templates for benchmarks. Making it to run vacuum, solvent and complex phases in that order.

## Motivation and context

 

## How has this been tested?

Tested locally

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Examples and benchmarks template input files now run vacuum, solvent and complex phases in order.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b477551c011e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
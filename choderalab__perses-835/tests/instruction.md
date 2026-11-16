# Issue

**Title:** Write out YAML file after parsing

## Description

## Description

Write out YAML file after parsing.

## Motivation and context

In order to improve reproducibility and quickly see which options were used in the simulation, it would be nice to be able to inspect the YAML that has all the options set as parsed.

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

Tested on GHA and locally manually.

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Write out YAML file after all options are parsed and set. Saved as YAML original file name + date + time. 
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5d50cbc30514`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** CLI - Making sure input ligand file exists

## Description

## Description

This change makes sure input ligand file exists when running simulations with the CLI. Avoiding freezing.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

Tested manually so far.

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Avoid freeze if ligand file is not found or does not exist when using CLI.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1aa695fb8738`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
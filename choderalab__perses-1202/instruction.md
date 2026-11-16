# Issue

**Title:** Allow specifying solvent model in input YAML file

## Description

## Description

These changes allow the specification of the solvent model through the input YAML file for the small molecule pipeline in perses. This is intended to be used via the perses cli.

## Motivation and context

Some users have expressed the desire to use different solvent models for perses simulations.

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Users can now specify solvent model for simulations using the ``solvent_model`` field in the input YAML file.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c99b6824e7dd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
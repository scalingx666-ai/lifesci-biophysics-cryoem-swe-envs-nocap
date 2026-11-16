# Issue

**Title:** CLI can now configure a platform to be used for the simulations

## Description

## Description

We can now specify the platform to be used for computations through CLI and YAML file in a consistent way. 

## Motivation and context

Up to this point the CLI and YAML input file were not sharing the `platform` option. The CLI only tested a given platform but wasn't "forcing" it in the simulation.

 

## How has this been tested?

Tested locally with CLI examples

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Command line interface (cli) can now set a platform for the multistate sampler computations.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `41a3caeecdb4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
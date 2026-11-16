# Issue

**Title:** Using the faster `LangevinDynamicsMove`

## Description

## Description

We should be using the faster [`LangevinDynamicsMove`](https://github.com/choderalab/openmmtools/blob/2e64980d86cf087f69e282927eebf2b41b73a3ea/openmmtools/mcmc.py#L1018) by default in our CLI workflow.

Tests and test systems were also updated to reflect the changes.

## Motivation and context

Increase performance by using the faster `LangevinMiddleIntegrator`.

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
CLI workflow for replica exchange now uses the faster ``LangevinMiddleIntegrator`` via the ``LangevinDynamicsMove``. Tests were updated to reflect the changes. 
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1dc9b869bfb6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Use energy and sampler context caches in simulations

## Description

## Description
Making perses use the new way of specifying context cache from `openmmtools`. This means setting up the simulations to use the energy and sampler context caches, respectively.

## Motivation and context

* Avoid cycling issue by specifying context caches. 
* Fixes simulation cooling down on resume.
* Minimizes systems before equilibration and production, for repex and sams.

 
 
 
 

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
* Using new streamlined way of dealing with context caches from openmmtools
* Avoid cool down of simulation on resume.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2ba82db1e214`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
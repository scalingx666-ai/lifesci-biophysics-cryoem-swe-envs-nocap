# Issue

**Title:** Remove hacks now that upstream has updated packages

## Description

## Description

Removed forks when installing openmm nightly 

## Motivation and context

Now that parmed has made a new release to support the openmm name change + we just cut (will be on conda-forge soon) a new openmoltools release, these CI hacks are no longer needed.

 

## How has this been tested?

Tested on CI

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
CI no longer requires forks for openmm namespace changes.  
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c1f5769d3610`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
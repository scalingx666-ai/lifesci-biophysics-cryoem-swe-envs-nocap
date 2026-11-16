# Issue

**Title:** Benchmark DeltaG plots now shift data to experimental DeltaG mean.

## Description

## Description

Currently, the DeltaG calculated vs experiment comparison plots subtract the mean from both axes, leading to nonsensical experimental axes:
![espaloma-plot_absolute-mean](https://user-images.githubusercontent.com/3656088/162648330-973a2de0-8847-4700-afef-47622a3b8020.png)
This PR adds back the experimental mean free energy to both axes so that the experimental range is correct:
![image](https://user-images.githubusercontent.com/3656088/162648198-88987447-b86c-4cdf-9636-830c94002398.png)

cc: 

## Motivation and context

Resolves what was originally suspected to be an issue with `openff-arsenic` but was just an incorrect usage:

 
## How has this been tested?

The attached plot has been generated.

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Benchmarking now produces a calculated versus experimental DeltaG plot that correctly centers both axes on the mean experimental binding free energy.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `40ef19dc8fe6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
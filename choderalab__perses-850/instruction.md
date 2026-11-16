# Issue

**Title:** Use mamba instead of conda

## Description

## Description

`mamba` tends to be faster than `conda` this PR changes our CI to use `mamba`

## Motivation and context

I'm working on using a selt-hosted runner on AWS, to save $ we need our CI to take less time. Instead of adding a GPU runner + switching to using `mamba`, I wanted to split the change into two PRs.



## How has this been tested?

Testing with CI right now :)

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Switch to mamba instead of conda
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `467c637366a5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
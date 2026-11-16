# Issue

**Title:** Not reassigning velocities on resume for small molecule pipeline

## Description

## Description

The small molecule pipeline (CLI) was reassigning velocities by default on resume. Changing this behavior to avoid equilibration issues when resuming simulations now that it is supported upstream 

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Now that upstream ``openmmtools`` is storing velocities on checkpoint, the small molecule transformation pipeline does not reassign velocities on resume ny defualt. Instead, the velocities are read from the checkpoint file.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `495c60f82527`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
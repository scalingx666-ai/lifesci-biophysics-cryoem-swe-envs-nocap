# Issue

**Title:** Realtime analysis interval to default to checkpoint interval

## Description

## Description

Changes the default for the `offline-freq` input parameter to match `checkpoint_interval`.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

Tested locally with CLI. Input yaml file in examples adjusted for it.

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Realtime analysis interval, ``offline-freq``, is now defaulting to ``checkpoint_interval`` when not specified in input parameter file.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d77ab23c5d82`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
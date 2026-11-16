# Issue

**Title:** Add more padding to fix issue #949

## Description

## Description

Fix issue #949 

## Motivation and context

Due to a change in openmm, we need to increase the padding for our simulation boxes.

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

Tested in CI

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Increase default padding in XXXX classes
```

I'm trying to increase the padding as little as possible in our tests/defaults to pass without increasing CI time significantly. So it is a bit slow/some trial and error to find that sweet spot.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `06bb0f482c4b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
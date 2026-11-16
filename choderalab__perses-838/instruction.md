# Issue

**Title:** Feat/add docker support

## Description

## Description

<!-- Describe your changes in detail. -->

This PR adds a docker file that we will maintain as an official docker image.

## Motivation and context

This is a first draft, we can optimize the container size and build time later. I would like to get it into our users hands so we can iterate on it.

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->

I've tested this image locally by building it, then starting a shell in the container, cloning the `perses` repo, then running pytest. You should be able to play around with it, `docker pull choderalab/perses:0.9.1` (uploading the image may take some time, so if you get image not found, that's why).

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Official Docker image hosted on docker hub `docker pull choderalab/perses:0.9.1`
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f68e5780e0bb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
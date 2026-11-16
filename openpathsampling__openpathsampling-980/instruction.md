# Issue

**Title:** Add codecov.yml

## Description

Adding [config file](https://docs.codecov.io/docs/codecovyml-reference) for CodeCov. In particular, I'm hoping that I can get CodeCov to wait until the integration tests are done before adding its comment/status indicator. It's a little annoying to get the email (which, unlike the comment, doesn't update) saying CodeCov hates the PR after only the minimal test suite has come in. Also, I keep seeing the red X on a PR and checking to see where the tests went wrong, when it's really that the tests are still running and CodeCov has incomplete info.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `82d5948dfe6f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
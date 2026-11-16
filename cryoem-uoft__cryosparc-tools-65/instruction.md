# Issue

**Title:** Project.dir and Job.dir should be properties

## Description

May I gently suggest that the directories of `Project`s and `Job`s should be a properties instead (i.e., `Project.dir` instead of `Project.dir()`).

I realize this is a breaking change so feel free to ignore, it's very nitpicky.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `161adca16b56`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
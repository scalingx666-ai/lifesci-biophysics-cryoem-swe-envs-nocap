# Issue

**Title:** Move check-openmm-rc off of main branch

## Description

We're currently using the `@main` branch of the `check-openmm-rc` GitHub action. This is not a best practice, because (1) it does not guarantee a stable API, and (2) a bad actor could change that action and potentially abuse our resources.

I'm not actually terribly worried about either (since I maintain the action, I would update here if I broke the API). However, @mikemhenry came across what seems to be a broken `@v1` tag on that, so I thought I'd try to figure out how to fix it here.

My goal is to get this to work with a "stable API" approach, where the `@v1` tag will work. The most secure approach is to pin to a specific commit (usually the commit associated with a specific tag). We probably won't worry about that here, because GH actions is well compartmentalized and it shouldn't be possible to steal secrets from within the `check-openmm-rc` action.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3893381f319f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
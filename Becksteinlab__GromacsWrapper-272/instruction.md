# Issue

**Title:** ci: try a simple ci.yaml (experiment)

## Description

When setting up to work on this repo, I found the setup very challenging. There is no setup script, so I worked based on what the ci.yaml does – and that contained a number of curveballs. Luckily things can be simplified and everything still works!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9ecbdea371d2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Fix OpenMM RC test workflow

## Description

Looks like this is failing for us due to some issues with an action no longer providing pip. This is intended to bring the OpenMM RC workflow more into line with our main testing workflow.

Assisted-by: Codex.app:GPT-5.4

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `644f3ed73748`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
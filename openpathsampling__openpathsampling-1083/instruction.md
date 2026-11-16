# Issue

**Title:** Fix website issues for Sphinx RTD Theme 1.0

## Description

(Note: this will already be fixed within this PR -- the `docs_deploy` branch generates and deploys documentation, so the only problems are already fixed.)

Essentially, our base theme for Sphinx just released a 1.0, and the way CSS was handled changed. This made our website pretty hideous. This fixes it.

See  for context.

This took a couple steps to get our full normal CSS back. Additionally, I switched the docs build to `mamba` for the same speedup seen in #1073.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ea605e8b8305`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
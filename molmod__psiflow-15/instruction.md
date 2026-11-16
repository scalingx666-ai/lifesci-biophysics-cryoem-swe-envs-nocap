# Issue

**Title:** Add copy button for code blocks in docs

## Description

This PR adds support for a copy button in the code blocks within the documentation, as described [here](https://squidfunk.github.io/mkdocs-material/reference/code-blocks/#code-selection-button). 

I also removed the `$` symbols from the code blocks, which made it difficult to copy and paste.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e011c2b17123`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
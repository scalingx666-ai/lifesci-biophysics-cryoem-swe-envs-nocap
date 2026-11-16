# Issue

**Title:** Rewrite features parser and config file generator

## Description

Process feature implication rules in topological order to guarantee that transitive chains are always evaluated. Add test case to check the features parser. Remove unused features checker.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2689af2cc0b0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
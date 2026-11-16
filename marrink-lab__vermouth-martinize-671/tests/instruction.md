# Issue

**Title:** improved backbone atom selection

## Description

With this PR, write the `[ macros ]` directive into the force field meta. This can then propagate useful things like atomnames for selectors. In this case, we use it to select backbones per force field when applying position restraints

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `35ee5479720e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
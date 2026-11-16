# Issue

**Title:** Bugfix: expanded symmetry partner chains weren't added to entities

## Description

This prevented things like writing out to mmcif file a bioassembly with expanded chains.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d622217782aa`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
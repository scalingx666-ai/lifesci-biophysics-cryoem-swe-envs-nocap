# Issue

**Title:** Add ability to set RNG in tests

## Description

Related to cpptraj , adds ability for pytraj to set what RNG will be used. This will make it so tests can pass if the default RNG is changed. 

@hainm sorry this is pretty rough code, you may want to make it a nicer interface eventually (like taking strings or enum instead of `int` ).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bfc7f9efcf7c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
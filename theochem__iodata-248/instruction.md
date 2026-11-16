# Issue

**Title:** Add support for bonds to SDF format

## Description

This PR adds support for the bond information in the SDF format (both load and dump). I've also made some minor improvements to conform better to the official format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d5523d161dff`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
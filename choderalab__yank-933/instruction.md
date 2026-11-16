# Issue

**Title:** Use receptor or solute as anchor molecule for imaging

## Description

This PR fixes a case where MDTraj raise an error when trying to guess the anchor molecule used for imaging, both in `extract_trajectory` and when computing restraint distances. It also skips molecule imaging for non-periodic systems.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d3d3812d11fc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
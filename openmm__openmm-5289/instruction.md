# Issue

**Title:** docs: document atom-to-particle index correspondence

## Description

. Documents that a System constructed from a Topology contains one particle per atom in the same order, so an atom's Topology index is also its particle index, addressing the maintainer's request: "Sounds good. I added the documentation label. A PR would be welcome!" The notes are added to ForceField.createSystem() and the Topology class docstrings as the reporter suggested.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `58749d9d8400`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
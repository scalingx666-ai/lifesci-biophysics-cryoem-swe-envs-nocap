# Issue

**Title:** Fix yaml combinatorial smiles + no filepath

## Description

There is a corner case which happens when the following conditions are met:
1. `smiles` or `name` is set in a `molecule`
2. `select: all` is set in the same molecule
3. `filepath` is *not* set in the same molecule

This results in a molecule expansion failure pre-Cerberus validation.

This PR ignores this specific set of conditions to allow the rest of the
expansion to continue unhindered, and lets Cerberus trap the schema
failure later on more gracefully.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5b5363ff24c2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
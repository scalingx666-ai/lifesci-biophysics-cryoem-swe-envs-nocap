# Issue

**Title:** Relax path with harmonic Bonds

## Description

### PR Summary: 
A quick change to path.relax() function to handle systems with incorrect bond lengths. WCAFENE style bonds in hoomd just fail if the length is > R0 (set to 1.5x the bond_length argument). In order to better handle systems with large variance in bond_lengths, also allow to pass the btype argument and set a harmonic bond style.

### PR Checklist
------------
 - [x] Includes appropriate unit test(s)
 - [x] Appropriate docstring(s) are added/updated
 - [x] Code is (approximately) PEP8 compliant
 - [x] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c4da8b82674b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
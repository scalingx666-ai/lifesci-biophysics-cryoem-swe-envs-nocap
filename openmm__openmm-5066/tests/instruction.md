# Issue

**Title:** Add Lipid21

## Description

Adds Lipid21 from   (Presumably we shouldn't merge this PR before that one.)  In addition to adding Amber19 to the Modeller test, I've also manually checked that the updated `amber19-all.xml` can be used to add membranes of all Modeller-supported types, and to set up a stable membrane protein simulation.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d8798cb3db9c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
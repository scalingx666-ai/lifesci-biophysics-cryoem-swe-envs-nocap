# Issue

**Title:** Overhaul and upgrade PDB parser

## Description

This reworks the PDB parser to use the machinery in parser_utils. In addition, adds CLI support for `-ignh` (drop hydrogens from input) and `-model n` (allow selection of specific models). Also implements half support for altloc atoms. The full solution would involve adding a CLI argument for it, but you probably want this per residue, and I'm unsure 1) what the syntax should look like and 2) how to propagate/implement it. Instead I've made it so we always take model A and give a warning for every atom with a B conformation.

 as it contains the same change.

Sem-Ver: feature

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d68e815c5d9d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
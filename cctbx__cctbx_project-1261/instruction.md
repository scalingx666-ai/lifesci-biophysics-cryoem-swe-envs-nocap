# Issue

**Title:** undowser2: keep restraint objects when rebuilding models

## Description

undowser2 rebuilds its models with `restraint_objects=None`. For a ligand the
monomer library lacks, restraints supplied by the caller (a user CIF, or ones
built from the CCD) are dropped, the ligand's hydrogens have no bonded parent,
and probe2 refuses the whole model, so water validation fails (7jr5). This
passes the model's restraint objects through both rebuilds.

Split out of `probe2-reuse-processed`; t96 with it gave no new failures
against master.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2876a8f5dd47`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
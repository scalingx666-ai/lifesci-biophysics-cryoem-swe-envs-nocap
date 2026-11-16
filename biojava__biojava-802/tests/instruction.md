# Issue

**Title:** Assembly chain ids for cases with composed operators in assembly expansion

## Description

A solution to problem described in #801. 
* It assigns chain ids containing all operator ids, e.g. for chain id `A` where operators 5 and 61 have been applied, it assigns new chain id `A_5x61`
* A new test is added to check correct behaviour in cartesian product `_pdbx_struct_assembly_gen.oper_expression`  cases
* Some cleanup removing jama objects from `PdbxStructOperList` mmcif model bean
* For consistency, the `Subunit` objects used in symmetry detection now use asym_ids instead of author_ids.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `edd755450132`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
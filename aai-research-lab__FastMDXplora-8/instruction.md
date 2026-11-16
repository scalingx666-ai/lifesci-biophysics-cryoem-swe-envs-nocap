# Issue

**Title:** Feature Addition Permissive Options Passthrough

## Description

Introduce OptionsForwarder utility to centralize alias handling, permissive kwargs forwarding, and strict-mode enforcement; export helpers via utils.__init__.
Update RMSD/RMSF/RG/SASA/SS/HBonds analyses to register alias maps, filter unknown options, and honor strict.
Ddd new features (RMSF per-residue aggregation, SASA sphere-point override, HBonds water exclusion and dynamic kwargs).
Wire strict-mode flag through analysis.analyze and surface --strict CLI switch so users can opt into hard failures on unsupported options.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `772b422b5e4b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
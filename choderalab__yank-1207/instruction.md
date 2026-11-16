# Issue

**Title:** Added support for CHARMM-format input files (psf/pdb and a list of pr…

## Description

The new code allows a psf/pdb combination to used with phase1_path and phase2_path. An additional variable charmm_parameter_files allows any number of CHARMM prm/str files to be included. This commit should fulfill yank github issue  #760 .

One current limitation is that Yank doesn't recognize the CHARMM names for counterions (SOD, CLA). My workaround is to do the replacements "s/SOD/Na+/" and "s/CLA/Cl-/" in the psf, pdb, and toppar_water_ions.str files.

Periodic cell information must be included in the pdb file.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1bf0f24f02b0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
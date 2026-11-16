# Issue

**Title:** 2.1.0rc1

## Description

Prepares the release candidate of 2.1.

- Version `2.1.0rc1`.
- The release notes of 2.1.0 become those of 2.1.0rc1: an introduction, then the entries grouped by subject (lattice dynamics, elastic constants, dislocations, sheets, other additions and changes, fixes). The text of every entry is unchanged.
- Performance: the analyses measured again on a whole Triton node (two Xeon Gold 6248, 40 threads) with the code of this release, OVITO 3.17.1 and freud 3.6.1; new tables, figure and summary. Polyhedral template matching now takes less memory than OVITO's. The pages on files and builders keep their earlier measurements, which reach larger sizes.

The API snapshot is written at 2.1.0.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8e4d4a931066`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** CUSTOM MDCATH

## Description

This PR introduces a custom version of MDCATH while preserving the original functionality to ensure backward compatibility.

New Arguments:

- `source_file `and `file_basename`: these args enable users to create and train with their custom versions of the MDCATH dataset.
-  `numNoHAtoms` by default is `None`, max number of non-hydrogen atoms in the protein structure.
- Assertions have been added to ensure that automatic downloading is restricted solely to the original version of the MDCATH dataset.

These enhancements aim to provide users with greater flexibility in dataset handling and customization, aligning with diverse research needs.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f6c0c16932e5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
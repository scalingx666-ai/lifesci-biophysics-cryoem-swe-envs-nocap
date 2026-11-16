# Issue

**Title:** Update BioAlignments and BioSequences compat. Change LongAminoAcidSeq to LongSequence{AminoAcidAlphabet}

## Description

Update BioAlignments and BioSequences compat in Project.toml. 
Change LongAminoAcidSeq to LongSequence{AminoAcidAlphabet}. (new type)

## Types of changes

This PR implements the following changes:
_(Please tick any or all of the following that are applicable)_

* [ ] :sparkles: New feature (A non-breaking change which adds functionality).
* [x] :bug: Bug fix (A non-breaking change, which fixes an issue).
* [ ] :boom: Breaking change (fix or feature that would cause existing functionality to change).

## :clipboard: Additional detail

  - **Does your change alter APIs or existing exposed methods/types?**

Change LongAminoAcidSeq to LongSequence{AminoAcidAlphabet}. (new type)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `453c233a445c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
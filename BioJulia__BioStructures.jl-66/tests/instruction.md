# Issue

**Title:** Add bonding data for residues

## Description

This provides deterministic bonding between atoms in peptides that follow standard naming conventions. The bonding data is taken from OpenMM's ff14SB force field.

To encourage careful work, this is designed to error unless all atoms (including hydrogens) are present in the structure, and that HIS has been disambiguated as HIE, HID, or HIP. One can override this behavior by passing `strict=false` as a keyword argument to `MetaGraph`.

This doesn't really have a plan (yet) for how to handle disulfide bonds. They're not too hard to add manually, but it wouldn't hurt to have a utility for this. I don't know whether it's safe to assume that proximity means the disulfide bond is present.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `58456ce24654`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
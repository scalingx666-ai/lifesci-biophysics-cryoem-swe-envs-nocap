# Issue

**Title:** Specialize residue names

## Description

If you've added hydrogens to a structure, this supports the full information present in the `residuedata` tables imported from ff19SB:
- renames HIS to one of HID/HIE/HIP (using the protonation state from the added hydrogens)
- renames the amino- and carboxyl-terminal residues by prepending `N`/`C`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `98bfd752a3c2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
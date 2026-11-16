# Issue

**Title:** CDL: keep allow_poly_ca for every chain in generate_residue_tuples

## Description

`generate_residue_tuples` rebuilds its `LinkedResidues` accumulator after each
chain without passing `allow_poly_ca`, so CA-only residues were linked in the
first chain only. This passes it through, as the first construction already does.

No caller in the tree passes `allow_poly_ca=True` today. `tst_poly_ca_multi_residue.py`
gains a two-chain CA-only case, which fails without the fix.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e2c4c2ee2946`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
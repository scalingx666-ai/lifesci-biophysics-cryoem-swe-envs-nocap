# Issue

**Title:** Ensure permutation equivariance of `oe_assign_charges`

## Description

Intent: ensure `oe_assign_charges(mol)[perm] == oe_assign_charges(mol[perm])` for any renumbering of atoms.

(related to  )

---------------

Initial approach: Apply `CanonicalRankAtoms` to rdkit mol before converting to oemol, calling into omega, .... This does not resolve the issue. (This confirms sensitivity to other factors, such as bond ordering.)

Revised approach: Toggle omega setting `SetCanonOrder` from `False` to `True`. (And recover initial atom ordering using `{Set/Get}MapIdx` in calling context.)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8982b819cf93`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
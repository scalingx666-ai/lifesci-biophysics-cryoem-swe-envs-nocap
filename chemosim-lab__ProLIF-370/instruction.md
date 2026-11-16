# Issue

**Title:** Fix standardizer, hydrogen count, hbond SMARTS, split_mol_by_residues and residue fragmentation

## Description

Temporary fix for issue #358 when using the `MoleculeStandardizer`

- Assign hybridization after the residues have been "fixed" by the standardizer. Inferred from the geometry of the parent "whole" structure.
- Fixes an invalid H count in both the standardizer (caused by the standardization of the residues during templating) and the residue fragmentation (only occurred when identical residues were bonded together and had to be split)
- Fixes the implicit HBAcceptor SMARTS by removing the valence query on the cut atom. Also replaces the explicit degree `D` with total degree `X` for readability, and addresses proline that wasn't covered by the exclusion before.
- Brings the proline exclusion to the explicit HBAcceptor pattern as well

In addition to these fixes, a few more issues were uncovered and fixed while debugging the problem:
- Fixes an issue in `split_molecule` that occurred when both fragments were spanning multiple components across different chains
- `use_segid` wasn't fully propagated down the line

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `19f180021838`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
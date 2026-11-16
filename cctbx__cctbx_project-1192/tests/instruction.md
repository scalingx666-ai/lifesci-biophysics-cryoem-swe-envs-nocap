# Issue

**Title:** Add disulfide bond drawing to kinemage output

## Description

## Summary

Adds disulfide (SS) bond drawing to the MolProbity kinemage output. CYS SG–SG bonds
are detected from the geometry restraints and drawn as yellow vectors in the sidechain
master group, matching the historical MolProbity/prekin `@vectorlist {SS}` format. This
restores SS-bond rendering that the kinemage module previously lacked.

## What changed (`mmtbx/kinemage/validation.py` only)

- New helper `_build_ss_bond_list(bond_proxies, i_seq_name_hash)` — returns the list of
  `(i_seq_0, i_seq_1)` CYS SG–SG pairs found in the bond proxies.
- `get_kin_lots` gains `ss_bonds=`/`sites_cart=` params and, when given, emits a
  `@vectorlist {SS}` (yellow, `master= {sidechain}`) for that chain. Each bond is keyed to
  the chain of its first atom to avoid drawing it from both ends.
- `_build_kinemage`, `make_multikin`, and `export_molprobity_result_as_kinemage` thread the
  SS-bond list and `sites_cart` through (alongside the existing
  omega/rna_puckers/cablam/probe_dots params).

This also lays the foundation (`_build_ss_bond_list` + the `ss_bonds`/`sites_cart` plumbing)
that the upcoming ribbon-rendering and `build_kinemage_from_model` work builds on.

## Testing

- `mmtbx/kinemage/tst_validation.py` — all groups pass.
- `phenix_regression/validation/tst_kinemage.py` — passes.
- Functional check: `phenix.kinemage` on a 4-disulfide structure (1nxb) renders all four
  SS bonds (CYS 3–24, 17–41, 43–54, 55–60); probe-dot output is unchanged.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a824b2c2defa`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
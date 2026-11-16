# Issue

**Title:** Fix IndexError when MTRIX/BIOMT expansion runs out of 2-character chain ids

## Description

`mmtbx.model.manager._expand_symm_helper` handed out new chain ids by index from `all_chain_ids()`, a fixed list of 3906 one- and two-character ids. A model with many chains and many operators (e.g. 112 chains x 106 `_struct_ncs_oper` rows marked `generated`) exhausts it and fails with `IndexError: list index out of range`.

Changes:
- `iotbx.pdb.utils.chain_id_generator(used_ids)`: yields the same 1- and 2-character ids in the same order, then continues with 3-, 4-, ... character ids, which are valid in mmCIF.
- Used in model MTRIX/BIOMT expansion, `cctbx.crystal.super_cell`, `iotbx.ncs` user-selection validation and `mmtbx.solvent.map_to_water` (new water chain id no longer silently falls back to `zz`).
- `phenix.pdb.mtrix_reconstruction` / `biomt_reconstruction` write mmCIF when the expanded model no longer fits PDB format, instead of an empty `.pdb`.
- Removed a dead `all_chain_ids()` call in the mmCIF writer.

Tests: new `exercise_many_chains_many_operators` in `tst_model_biomt_mtrix_2.py` (38 chains x 106 operators, failed with the IndexError before), generator check in `iotbx/pdb/tst_utils.py`. Existing MTRIX/BIOMT, NCS, super_cell, mmCIF writer and douse tests pass; chain id ordering for the first 3906 ids is unchanged.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b897de9a4d9f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
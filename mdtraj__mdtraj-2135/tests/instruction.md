# Issue

**Title:** Fix Topology.copy() bond remapping and PDBxFile pathlib support

## Description

## Summary

This PR fixes two bugs:

### 1. `Topology.copy()` bond remapping ()

`Topology.copy()` creates new chains, residues, and atoms, but `add_bond()` was called with atom objects from the **original** topology. 

This caused failures (when I was using 1.11.0) in operations that use chain/residue objects as dict keys (e.g., `save_cif`), because `Chain` lacks a custom `__eq__` — so hash matches but identity-based equality fails. Although #2103 fixes the indexing problem of `save_cif`, it does not fix the bond issue, which is addressed in this PR.

**Fix:** Remap bond atoms to the new topology via `out.atom(a1.index)`.

### 2. `PDBxFile` pathlib support (relates to #1663)

`PDBxFile.__init__` and `writeFile` only accepted `str` paths. Passing a `pathlib.Path` caused `TypeError: 'PosixPath' object is not iterable`.

**Fix:** Accept `os.PathLike` in addition to `str`, wrapping with `str()` before passing to `open_maybe_zipped`.

### Tests

- `test_copy_bonds_reference_new_atoms` — verifies bonds in copied topology reference new atoms and `traj[0].save_cif()` works
- `test_load_cif_with_pathlib` — verifies CIF load/save roundtrip with `pathlib.Path`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bfdce156d050`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
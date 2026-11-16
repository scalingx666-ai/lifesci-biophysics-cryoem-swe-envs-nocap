# Issue

**Title:** reduce2/clashscore2: preserve user restraint objects for custom ligands

## Description

## Summary

Fixes the reduce2 → clashscore2 pipeline for custom ligands that carry user-supplied
restraint CIFs (e.g. from eLBOW), where the user's restraints were being lost:

1. **`reduce_hydrogen.py`** — in `place_hydrogens`, load any user-provided restraint CIF
   objects into `mon_lib_srv` before querying residues. Otherwise `mon_lib_query()`
   generates `auto_*` CIF objects with empty `type_energy` values that overwrite the
   user's entries.
2. **`clashscore2.py`** — preserve `restraint_objects` when rebuilding the model from a
   hierarchy (both the initial rebuild and each trimmed submodel), instead of hardcoding
   `restraint_objects=None` and silently dropping them.

### Scope / why it's small

This is the low-risk half of a larger custom-ligand fix, split out deliberately. Both
changes are **no-ops for models without user restraints**: `get_restraint_objects()`
returns `None` in that case, so clashscore2 passes the same `None` it did before, and the
new reduce_hydrogen block is guarded by `if ro:`. So standard models are byte-for-byte
unaffected.

The remaining two hunks from the original fix touch core interpretation code
(`model.manager.select()` propagating `stop_for_unknowns`, and a prime/star atom-name
synonym fallback in `pdb_interpretation`) and have a much broader blast radius, so they
are held back for separate maintainer review rather than bundled here.

## Testing

- `mmtbx.clashscore2` on 1nxbH: **byte-identical clashscore to master** (294.57 both
  ways) — confirms the rebuild change is a no-op when no user restraints are present.
- `mmtbx/hydrogens/tst_add_hydrogen_{1,2,3,5,7,10,11}.py`: all pass (reduce2 H-placement
  unaffected on the no-restraints path).
- `libtbx.find_clutter` and `py_compile` clean on both changed files.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `27463e2b09d2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
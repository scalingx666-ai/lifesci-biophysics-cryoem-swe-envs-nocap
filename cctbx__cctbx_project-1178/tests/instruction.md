# Issue

**Title:** Speed up clashscore2: make redundant 2nd probe2 run optional (~2x)

## Description

## Summary
Speed up `clashscore2` ~2× by making the redundant second `probe2` run optional, plus two small efficiency cleanups. Output on the default path is unchanged.

1. **Gate the second probe2 run behind a new `save_probe_output` phil (default `False`).** `probe_clashscore_manager` previously *always* ran probe2 a second time to produce full non-condensed output with VDW contacts ("for Coot"), stored it in `self.probe_json`, and never consumed it. That run is now skipped unless `save_probe_output=True`, which roughly halves runtime. The flag is threaded `programs/clashscore2.py` → `clashscore2.__init__` → `probe_clashscore_manager`.
2. **Classify water by residue group, single pass.** The non-water atom count now iterates `residue_groups() → atom_groups() → atoms()`, skipping whole water groups once instead of calling `common_residue_names_get_class` per atom; the `natoms_b_cutoff` tally is folded into the same pass (was a second full atom loop).
3. **`used = set()` for clash dedup** (was a list), and removed a dead `id_str_no_atom_name()` assignment that was immediately overwritten.

## Correctness
Verified `mmtbx.clashscore2` on a hydrogenated model (1nxb) before/after: **identical clashscore (293.67) and a byte-identical bad-clashes list** with the default `save_probe_output=False`. Wall time dropped from ~7.8s to ~5.5s.

Style gates pass locally (`libtbx.find_clutter`, `py_compile`). One now-unused `import re` was removed — the old crude clutter check had been treating the substring in a `we're` comment as a use; rewriting that comment exposed the genuinely dead import.

## Notes
Touches `mmtbx/programs/clashscore2.py` and `mmtbx/validation/clashscore2.py`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b6f231e03a57`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
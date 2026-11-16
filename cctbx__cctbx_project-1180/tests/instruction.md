# Issue

**Title:** Refactor mmtbx/kinemage/validation.py: fix bugs, dead code, consolidate entry points, probe2 dots

## Description

## Summary
Foundational refactor of `mmtbx/kinemage/validation.py`: fix several bugs, remove dead code, consolidate the two kinemage entry points onto a shared builder, and migrate probe-dot generation from shelling out to the legacy `reduce`/`probe` executables to the in-process `reduce2`/`probe2` Python API.

Key changes:
- Consolidate `make_multikin` and `export_molprobity_result_as_kinemage` onto a shared `_build_kinemage`.
- Migrate `make_probe_dots` to the probe2 Python API (no more external `reduce`/`probe` subprocess).
- Fix correctness bugs: an altloc `==`-vs-`=` assignment, an `is not` string comparison that should be `!=`, duplicate geometry-deviation overlays (emitted per chain segment instead of per chain id), and a duplicate `{length dev}` line in the footer.
- Remove dead code (`add_fan`, `add_spring`, `get_residue_bonds`, `get_angle_outliers`, `get_bond_outliers`) and extract readable helpers out of `get_kin_lots`.

## Probe-dot correctness
The probe2-API migration originally had two bugs that made `make_probe_dots` throw and silently return empty (a bare `except` swallowed it): the `Optimizer` was handed top-level params instead of `params.probe`, and it called a nonexistent `DataManager.set_default_output_dir()`. Both are fixed here (the latter via `CCTBXParser`, matching `clashscore2.py`), so `run()` again emits probe/H-bond dots.

Verified that the refactored `run()` produces the **same 69 greentint H-bond dots** as the previous (legacy-engine) code on the standard His31–Asp70 H-bond test case.

## Tests
- New `mmtbx/kinemage/tst_validation.py` covering the extracted helpers, altloc handling, `build_kinemage`, `make_multikin`, and that the deleted functions are gone. Passes locally.
- The companion `phenix_regression/validation/tst_kinemage.py` (made engine-agnostic in a paired MR) passes against this branch.

## Notes
Large but cohesive single commit (the refactor is internally interdependent). Touches only `mmtbx/kinemage/validation.py` and its new test.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `24170591a110`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
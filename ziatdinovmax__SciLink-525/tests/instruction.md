# Issue

**Title:** Accept common axis-range metadata synonyms in hyperspectral analysis

## Description

## Summary

Hyperspectral metadata written outside SciLink — for example an instrument sidecar that records the Raman-shift axis as `spectral_axis: {min, max, units}` — used to make the analysis fail immediately with *"Axis precondition failed: Energy axis information is required"*, even though the axis range was right there under a slightly different name. The scientist's metadata was fine; only the spelling differed from SciLink's canonical `energy_range: {start, end}`.

With this change, the hyperspectral agent recognizes these common spellings and translates them automatically before analysis starts. Metadata that already uses the canonical form behaves exactly as before — this only turns a class of hard failures into working runs. Nothing changes for LLM-based metadata extraction from prose; this is a deterministic translation of unambiguous shapes.

Observed live on collaborator micro-Raman mapping datacubes, where every sidecar uses the `spectral_axis` spelling and every direct `analyze(system_info=...)` call died at the precondition.

---

## Technical details

Three coordinated edits, all in the existing Tier-1 machinery:

1. **`normalize_metadata_dict`** (`metadata_converter.py`): after the flat-key aliases, fold nested axis-range synonyms — `spectral_axis` with `min`/`max` (or `start`/`end`), and `energy_range` spelled with `min`/`max` — into canonical `energy_range.start/end` (+ `units` if absent). Canonical always wins; the source key is preserved (unrecognized keys are kept, and it may carry context such as the axis name).
2. **`_describes_spectral_imaging`**: a declared `spectral_axis` dict now counts as describing spectral data. Without this, a dict whose prose misses the word list (e.g. technique `"Raman microscopy"` for a mapping datacube) fast-paths through `check_schema_conformance` and the fold never fires.
3. **`HyperspectralAnalysisAgent._handle_system_info`**: override applies the Tier-1 normalizer on top of base handling — one seam covering every metadata consumer on the agent (`analyze` pipeline, interpretation prep, precondition). No-op for canonical dicts; scoped to the hyperspectral agent, which is the one with the hard axis requirement.

**Backward compatibility is by construction**: the fold fires only where today's behavior is a hard failure. `tests/test_metadata_axis_synonyms.py` asserts the no-op (canonical dict returns the *same object*, unmutated), canonical-wins when both shapes are present, the literal failing sidecar shape resolving end-to-end through `create_axis`, no half-fold on a half-specified range, and the agent seam. Adjacent suites (`test_metadata_conformance_energy_axis`, `test_system_info_handling`, `test_signal_axis_vector`, offline hyperspectral suites) pass; the one failure in `test_hs_dynamic_records.py::test_retry_ladder_reaches_hot_and_succeeds` reproduces on clean `main` and is unrelated.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `faf3423aea37`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
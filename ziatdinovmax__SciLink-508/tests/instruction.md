# Issue

**Title:** Fix fusion report overwrite on resumed sessions; use exact wavelength tables for hyperspectral axes

## Description

Two independent fixes found during a live multimodal fan-out session.

**1. A resumed meta session could silently overwrite its earlier fusion report.**
If you restored a meta session from a checkpoint and ran a second fan-out + fusion, the new `fusion_report.html`/`.json` landed in the same `fusion/01/` folder as the first one and replaced it. Your earlier cross-dataset synthesis was simply gone. Now every fusion gets its own folder, and fusion results survive session restores.

**2. Hyperspectral band positions were shifted by up to ~6 nm.**
When a dataset's metadata preserves the spectrometer's exact wavelength table (which is usually *not* evenly spaced), SciLink still rebuilt the axis as an evenly spaced ramp between the two endpoints. On a real 260-channel visible/NIR camera that ramp is up to 5.9 nm wrong mid-spectrum — so every reported peak/dip center inherited a few-nm bias. SciLink now uses the exact per-channel wavelengths whenever the metadata provides them (`axis_spec.axis_2.samples`, or a top-level `wavelengths_nm` / `wavelengths` / `energies` list). Nothing changes for metadata that only gives a start/end range.

---

<details>
<summary><b>Technical details</b></summary>

**Fusion-dir collision (`meta_agent/fanout.py`)** — two composable defects:
- `fuse_delegations` appended its `mode="fusion"` ledger entry but never checkpointed, so `restore_checkpoint=True` sessions restored a ledger with no fusion entries. Fixed by calling `_auto_checkpoint()` right after the append (mirrors `_close_delegation`).
- The output dir number was derived only from the in-memory fusion count (`fusion/{n:02d}`). Added a skip-existing-dirs allocator under `_fanout_lock`, so even sessions resumed from pre-fix checkpoints cannot re-allocate an existing dir.
- Regression test: section 3b of `tests/test_meta_fanout_robustness.py` simulates the stale-ledger restore and asserts the checkpoint carries the fusion entry, the second fuse lands in a new dir, and the first report is byte-identical. Suite: 47/47.

**Exact axis samples (`metadata_converter.py` + consumers)**:
- `resolve_axis_spec` passes through `axis_spec.axis_2.samples` (numeric sequence, len ≥ 2, finite) or adopts the first recognized top-level vector (`wavelengths_nm`, `wavelengths`, `energies`); `start`/`end` default to the first/last sample so all range-only consumers keep working.
- New `signal_axis_values(axis_info, n_channels, logger)` helper returns the exact vector when its length matches the data, else warns and returns `None` — callers keep their existing `linspace(start, end)` / channel-index fallbacks byte-identical.
- Wired at all three axis-construction sites: `eels.create_axis` (the canonical builder, also behind `create_energy_axis`), the per-pixel codegen's `state["energy_axis"]` in `hyperspectral_controllers.py`, and the script-bank fingerprint in `hyperspectral_analysis_agent.py`. Downstream consumers (`convert_energy_to_indices`, band-flux binning, plots) already operate on the axis array and handle nonuniform spacing.
- `check_schema_conformance` accepts a sample vector as a resolvable energy axis (previously only `energy_range`/`axis_2` start+end).
- Tests: `tests/test_signal_axis_vector.py` (pass-through, top-level adoption, precedence, length-mismatch fallback, legacy linspace path unchanged, conformance); existing hyperspectral suites (`test_metadata_conformance_energy_axis`, `test_spectral_unmixer_mask`, `test_hyperspectral_session_fixes`, `test_hyperspectral_qc_scalar_fixes`) all pass (12 + 64). Verified on a real 704×704×260 cube sidecar: agent axis now equals the preserved table exactly; the old reconstruction was 5.89 nm off at ~692 nm.

</details>

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3ad9ed12af1c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
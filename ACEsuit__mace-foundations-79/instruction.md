# Issue

**Title:** mace_polar_1: pass charge/spin keys and use REF_energy/REF_forces

## Description

## Problem

`mace_polar_1/mace-polar-1-medium.sh` and `mace-polar-1-large.sh` don't pass the charge/spin keys, and set `--forces_key='forces'`. On OMol `.aselmdb` data this means:

- **Charge/spin:** OMol stores charge/spin under `data["charge"]` / `data["spin"]` (surfaced as `atoms.info["charge"]` / `["spin"]`). Without `--total_charge_key`/`--total_spin_key` the defaults look for `total_charge`/`total_spin`, which are absent, so `total_charge`/`total_spin` fall back to `0` / `1` — **every system trains as a neutral closed-shell singlet**.
- **Forces:** `LMDBDataset` populates forces under `REF_forces`. Once mace honors CLI key overrides (companion PR **ACEsuit/mace#1516**), `--forces_key='forces'` selects the empty `atoms.arrays["forces"]` and forces load as **zero**. Reading `REF_forces` is correct. (On today's mace this override is ignored and forces happen to fall back to `REF_forces`, so this change is also correct on current mace.)

## Fix

Set the key block to match `mace_omol/mace-omol.sh`:

```diff
-    --energy_key='energy' \
-    --forces_key='forces' \
+    --energy_key='REF_energy' \
+    --forces_key='REF_forces' \
+    --total_charge_key='charge' \
+    --total_spin_key='spin' \
```

Applied identically to the medium and large scripts.

## Companion PR

This depends on **ACEsuit/mace#1516**, which makes `LMDBDataset` honor the CLI key specification (currently the charge/spin flags are silently ignored for `.aselmdb`/`.lmdb` datasets). That PR also documents the forces coupling above.

Order of merge does not matter for correctness, but both are needed for the polar-1 recipes to train with the correct charge/spin conditioning and non-zero forces.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c6d6d9d1c1a7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
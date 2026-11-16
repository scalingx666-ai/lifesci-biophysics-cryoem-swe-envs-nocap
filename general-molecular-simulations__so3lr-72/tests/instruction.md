# Issue

**Title:** tune-ewald: tune PME on the flash engine

## Description

`so3lr tune-ewald --kspace-electrostatics pme` now runs on the flash engine whenever flash takes the structure. On a 32,512-atom POPC bilayer (A100 40 GB), M and L each ran out of memory on the neighbour-list path. Each now finishes in about 3.5 min wall-clock.

## Why it ran out of memory

- **M** ran out in the timing loop: each neighbour-list force call needed about 33 GB.
- **L** ran out earlier, in the single model call that gives the partial charges. Its attention tensor is float64 even under `mixed`.

## What changed

**Flash route** (`so3lr/cli/tune_ewald.py`, `so3lr/flash/{engine,check}.py`)
- `--engine auto|flash|standard` on `tune-ewald`, as in MD. `auto` uses flash for PME whenever `check` lets the structure through (new task `"tune"`, which takes PME only). Ewald, P3M and the `cutoff` modes stay on the neighbour lists. `--engine flash` with them is an error.
- Charges come from the new `FlashSO3LR.observables(R)`, which returns `(q, a0, c6)`: the staged forward plus the heads, with no long range and no backward pass.
- Timing builds one flash engine per (cutoff, PME mesh shape). Cutoff and mesh are baked into the engine. The smearing, and spacings that round to the same mesh, change no work (#42 item 8), so they reuse the timing.
- Flash and the neighbour lists take the same charge and spin from settings, without the charge-embedding overrides. The CLI now passes `--spin` to `tune-ewald`; it was dropped before.
- `energy_forces_strain_staged` and `observables` share one rebuild loop, `_staged_rebuilding`, taken out of the former.

**Estimator** (#42)
- Item 7: the real-space kernel error is measured on the pairs of up to 2048 sampled atoms (`SamplePairs`), not every neighbour pair. The pair forces come from one module-level jitted function, with `r_max` and the kernel parameters traced.
- Item 2: the PME k-space estimate uses the spacing of the mesh jax-pme actually builds, padded to a power of two. Estimates come out about 4-9× lower.
- `mesh_lo`/`mesh_hi` now mean 2**m points along the shortest side (`pme_mesh_spacing`). The defaults moved from 4-6 to 5-7, which tries the same meshes. A YAML that sets these keys gets meshes half as fine, and the log now lists the meshes tried.
- Item 4: `_inclusive` no longer steps one past the end of a range that is not a whole number of steps.
- The recommendation is the most accurate candidate within `accuracy` whose timing is within 3 % of the fastest. On flash the mesh is almost free next to the model (0.0764 vs 0.0799 s), so taking the minimum timing picked σ 3 at 9.7e-4 over σ 2 at 3.8e-4.

## Validation (MeluXina)

- CPU: `test_tune_ewald_error_bounds`, `test_md_observables`, `test_cli`, `test_flash`, `test_engine` — 238 passed, 76 skipped, 1 failed (`test_flash_runs_chargeseparation_charge_sites_as_perform_md`, a PySAGES setup error that fails on main too)
- GPU (A100): `test_observables_match_neighbour_lists[so3lr-tauri-m-2606, so3lr]` (flash charges = neighbour-list charges in float64), `test_tune_ewald_on_flash` (same totals as `--engine standard`, rtol 1e-3, at most 2 timed engines), `test_atoms_in_any_order`, `test_narrow_neighbour_rows_widen` — all pass
- POPC 32,512 atoms, mixed precision, lr 10 Å, M and L on flash: both finish in 186 s wall-clock (job included); each recommends lr 10 Å, σ 2 Å, the finest mesh, estimated error 3.8e-4 (M) / 3.6e-4 (L) eV/Å. Flash force calls: M 0.077 s, L 0.245 s

New tests:
- `test_sampled_pair_forces_match_autograd` (both kernels, float64)
- `test_pme_candidate_spacings_give_their_mesh`
- `test_candidate_ranges_stay_within_their_bounds`
- `test_recommendation_breaks_timing_ties_by_error`
- `test_tune_task_takes_pme_only`
- The two GPU tests above

## Not in this PR

- **Ewald k-cutoff (#42 item 1) and the P3M estimate (item 3)** are not done.
- **Dispersion-cutoff tuning** still builds full neighbour lists.
- **No fallback after the first engine.** If a later engine in the grid fails to build, the run stops instead of switching to the neighbour lists. MD has the same limit once it has started.
- **Rare species can be missed.** A 2048-atom sample may contain none of a rare ion, so the real-space error is an estimate for the bulk. Smaller systems use all atoms.
- **The float64 attention tensor in L under `mixed`** (the neighbour-list path's memory) is not fixed here.

Docs:

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `239d22834517`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
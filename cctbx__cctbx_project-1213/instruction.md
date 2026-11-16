# Issue

**Title:** Add configurable parallelism, convergence, and a NumFreq Hessian to orca_manager

## Description

Adds configurable options to `orca_manager`, mirroring the `xtb_manager` Hessian work in #1210 so the two managers expose a consistent public API.

- `%pal nprocs N` when `nproc > 1` (previously `nproc` was ignored, so ORCA always ran serial).
- A settable `optimisation` keyword (default `LooseOpt`).
- `%geom` convergence overrides via `geom_convergence`: `None`, a `GEOM_CONVERGENCE` preset name (e.g. `'looser'`), or a dict of `Tol*` settings, injected ahead of the `Constraints` sub-block.
- An `exe` attribute that `get_cmd` uses ahead of `$PHENIX_ORCA`.
- A revived `get_engrad` (the commented-out version called a nonexistent `print_timings`; its one caller, `qm_manager.main`, was broken).
- `run_hessian(sites_cart=None, hessian_atoms=None, partial=True, log=None) -> path`: an ORCA `NumFreq` (optionally partial over selected atoms) returning the `.hess` path. Public surface matches `xtb_manager.run_hessian` (#1210); internals stay orca-idiomatic (one monolithic `.in`).

Backward-compatible: with defaults (`nproc=1`, no `optimisation`/`geom_convergence`/`exe`), the generated input is byte-identical to before. The one behavior change is that `nproc>1` now parallelizes ORCA (was silently serial) - a bugfix.

Tests: `mmtbx/geometry_restraints/tst_orca_manager.py` (registered in `mmtbx/run_tests.py`; no ORCA binary needed).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4f9147ab9d3e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
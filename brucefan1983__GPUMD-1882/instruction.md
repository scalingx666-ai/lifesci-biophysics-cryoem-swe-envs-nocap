# Issue

**Title:** Allow unwrapped-only output in dump_xyz and dump_netcdf

## Description

### What changed

- Add `unwrapped_position_only` to `dump_xyz` and `dump_netcdf` to write only unwrapped coordinates.
- Preserve existing usage: no option writes wrapped coordinates; `unwrapped_position` writes both wrapped and unwrapped coordinates. The two unwrapped options are mutually exclusive.
- Document the new option and extend tests for coordinate output, input validation, and NetCDF append consistency.

```text
dump_xyz 15 dump.xyz unwrapped_position_only velocity
dump_netcdf 15 movie.nc unwrapped_position_only velocity
```

### What this improves

- Avoid redundant wrapped coordinates in large trajectories while keeping existing input files compatible.
- Mark unwrapped-only output with `position_mode=unwrapped` and reject incompatible NetCDF appends.

### Validation

- Full CUDA/NetCDF build passed on upstream `9cbf049b` plus this change.
- Related GPU tests: 582 passed, 184 skipped (unavailable/inapplicable model combinations and optional MDAnalysis dependency), 64 deselected. Covers periodic crossings, triclinic cells, groups, precision, compression, appending, argument errors, and velocity reinitialization.
- RST syntax and `git diff --check` passed. The full project test suite was not run.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9cbf049b24fd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
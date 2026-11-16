# Issue

**Title:** Make ParticleSlice getter return pid-sorted values

## Description

`get_particles_properties()` sorted gathered `(pid, value)` pairs by
ascending pid before returning them, but setters write values positionally
against the user-supplied `id_selection` order.  For any non-ascending
`by_ids([3,1,2])` selection the set/get round-trip silently returned
reordered data.

**Fix:** replace the sort-then-iterate with a `pid→value` unordered map
that returns values in the original `id_selection` (`pids`) order
(`src/script_interface/particle_data/ParticleSlice.hpp`).

**Regression tests:** `test_charge_roundtrip_non_ascending` and
`test_pos_roundtrip_non_ascending` added to `ParticleSliceTest` in
`testsuite/python/particle_slice.py`.

Fixes bug #4 of the adversarial bug sweep.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `724e8b8a8f3c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
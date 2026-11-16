# Issue

**Title:** fix(md): remove the k-space net force so a periodic box does not drift

## Description

Smooth PME (electrostatics and dispersion) spreads the charges on a mesh that is fixed in space. Its forces are therefore not translation invariant and sum to a small net force. jax-md zeroes the total momentum only at the start, so over a run that force builds up into a drift of the whole box, which an MSD then counts as diffusion.

- `jaxmd_utils.without_net_force` removes the mean force over the atoms (as AMBER's `netfrc`). The energy and the pressure are unchanged.
- It is on for MD and minimisation whenever a k-space term is on (`kspace_electrostatics` or `kspace_dispersion`). The new key `kspace_remove_net_force: false` turns it off, which reproduces the old behaviour.
- The flash engine does the same when PME is on; `flash/verify.py` builds its reference the same way.
- Single points keep the model's forces: `so3lr eval`, `So3lrCalculator`, `FlashCalculator` and training.
- On restart (both engines), the centre-of-mass velocity is removed, so a restart file written before this change does not keep drifting. This is skipped with `kspace_remove_net_force: false`, with `nvt-langevin` and `langevin_removecmmotion: false`, with fixed atoms, and when a PySAGES state is restarted.

Tests: `tests/test_kspace_net_force.py` (the settings, MD forces summing to zero with PME, flash single points keeping the model's forces, and restarts losing the centre-of-mass motion), and updated cases in `tests/test_pme.py`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `59198f64d9e3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
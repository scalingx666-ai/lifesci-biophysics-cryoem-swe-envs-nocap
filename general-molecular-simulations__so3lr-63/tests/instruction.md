# Issue

**Title:** Move jax-md to current upstream and skip jax 0.10.2

## Description

## Move jax-md to current upstream and skip jax 0.10.2

### jax 0.10.2 gives wrong energies on CPU

`uv.lock` on main installs jax 0.10.2. On CPU, 0.10.2 miscompiles a jitted, vmapped 3×3 matrix–vector product once there are more than about 16k rows (). jax-md's periodic displacements are such a product, so the neighbour-list potential returns wrong energies and forces with no error. For 64 waters in a 12.4 Å box (so3lr-tauri-s, float64) it gives −55.5 eV instead of −148.8 eV. The same code is exact on jax 0.10.1 and on GPU. `test_water` does not catch it.

This PR excludes 0.10.2 (`jax` and `jaxlib`; `jax[cuda12]` takes the same bounds), as upstream jax-md does; the lock moves to 0.10.1. An existing jax 0.10.2 GPU install is moved to 0.10.1: install with `[cuda12]` so the CUDA plugin moves with it, otherwise jax finds a 0.10.2 plugin it cannot use and runs on the CPU.

### jax-md

The pin moves from `a1618449` (on an old jax-md base) to the fork's `so3lr` branch, [`d27c351`](https://github.com/kabylda/jax-md/tree/so3lr): the commits so3lr needs, rebased onto current jax-md main. Upstream still lacks three of them:
- flexible-cell NPT (`npt_nose_hoover_flex`, `pressure_tensor_flex`, `npt_box_flex`);
- `minimum_cell_size_multiplier` for cell lists;
- `velocities=` in the integrators' `init_fn`.

The branch adds one more fix: an explicit `box=None` passed to a neighbour list means no box, as before.

Upstream jax-md now depends on `equinox` and `jaxtyping`, which the lock adds.

Changes in so3lr that follow from newer jax-md:
- **NPT neighbour lists whose cells became too small are now reallocated.** jax-md flags a cell list whose box has shrunk below its cells (`cell_size_too_small`), but `so3lr md` only reallocated on overflow, so the run went on with a list that misses pairs near the cutoff. The old jax-md sized every axis's cells from the shortest side, so the longer axes had room to spare. Newer jax-md sizes each axis on its own, which makes this much likelier: an 80 Å axis at a 4.5 Å cutoff goes from 37 % spare to 4.6 %, so about 5 % compression is enough. The cycle is now repeated with lists reallocated for the new box, as on overflow.
- neighbour lists store one fractional cell size per axis, which broke the cell-list log line in every MD run. The line now gives the actual cell size; before, it printed the cutoff.
- simulate states have a static `fix_center_of_mass` field, which the restart loader turned into an array (the PySAGES restart test failed).
- the PySAGES restart updates the neighbour lists with the box's lengths, as the MD loop does, not the 3×3 matrix.

### Tests

The CPU suite (all lanes, short and full) passes on jax 0.10.1 with this jax-md. On an A100 the jax-md, flexible-cell NPT, precision and flash NPT tests pass. `tests/test_jaxmd.py` adds a box shrunk below its cells: the reallocated list has every pair within the cutoff.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c892238b75d2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
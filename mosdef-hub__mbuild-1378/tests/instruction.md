# Issue

**Title:** Multiple improvments and fixes for path and simulation modules.

## Description

### PR Summary:

**Fix azimuthal sampling in random walks, plus PBC, backmapping and HOOMD updates**

This started as a fix for biased azimuthal sampling in `hard_sphere_random_walk`. A few related fixes ended up on the branch along the way, so it's a bit of a grab bag.

### Random walks

- Trial vectors are now drawn from a normal distribution instead of a uniform cube. The cube favored its diagonals, so the azimuth around the chain wasn't uniform. Heads up: the same seed gives different walks than develop now. Seeding is still reproducible on this branch, and the existing seed tests pass.
- PBC fixes:
  - The previous site gets unwrapped (minimum image) before the next step is built, so bond angles come out right across the boundary. This includes the first angle of a `link-linear` walk.
  - `get_second_point` wraps its candidates into the box.
  - All the overlap checks in `points.py` now get `pbc` and `box_lengths`.
- `TargetCoordinate` takes an optional `capture_radius`. Inside it, the bias weight ramps up to 1 as the walk gets close to `termination_radius`. That radius can come from a `WithinCoordinate` terminator, and `capture_radius="auto"` uses 10x the bond length. If that isn't larger than `termination_radius`, the walk raises an error before it starts and says to pass a float.
- When `include_compound` is used, biases now see the chain tip as `coordinates[-1]`. Before, the second step saw a compound particle there instead. This also fixes `TargetDirection` and `AvoidDirection`, which had the same bug on develop.
- The small-angle overlap warning respects `choice` sampler weights, skips zero-weight angles, and stays quiet below 0.5%.

### Backmapping

- New `box` argument. For a Compound it defaults to `system.box`. Bonds across the boundary are oriented using the minimum image, so fragments point through the boundary instead of across the box.
- Children are added in lists instead of one at a time. That's roughly 2x faster on large systems.
- Not in this PR: atoms near a face can still end up a little outside the box, and the output doesn't get a box set. That's part of a separate issue about consolidating PBC wrapping around freud.

### HoomdSimulation

- Frame handling:
  - Coordinates are matched to a centered or a corner box layout, per axis.
  - Systems that fit neither layout, or that only have a bounding box, are shifted in as one rigid piece instead of being wrapped atom by atom. Before this, a molecule far from the origin could get moved and split.
  - The frame is cached with the snapshot, so a reused snapshot round-trips correctly even if the coordinates now look like the other layout. A cached snapshot is only reused if its box still matches the requested box, so a new simulation after `box_update` doesn't shift the system.
- `nvt` and `box_update` accept `kT=[start, finish]` and ramp between the two values. A list or array that isn't exactly two values raises a `ValueError`.
- `ForcesHandler` scales tabulated angle and dihedral potentials. Before, they were dropped whenever a handler was used.
- New `speedup_by_moltag` option made accessible from mBuild simulation model. It is passed through to gmso.
- The path forcefield now leaves angle partners out of the WCA when the angle table covers every angle type. Develop only left out bonded pairs. Dihedral partners are left out when the dihedral table covers every type and angle partners are already left out. Without tables, only bonded pairs are left out, as before. The `ForcesHandler` docstring notes that scaling a table down, or the default `scale_periodic=0`, lets those beads overlap.
- A few `print` calls are now `logger.info`. The GPU fallback message is a `logger.warning`.

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b3a8db124bcf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
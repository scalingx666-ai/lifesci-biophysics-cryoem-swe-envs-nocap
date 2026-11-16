# Issue

**Title:** DislocationResult: densities, and some atoms only

## Description

Builds on #77 (and #76, #75), whose commits it contains until those merge.

- **Densities.** `DislocationResult.density` is the total length of the lines per volume of the box, in Å⁻²; multiply by 10²⁰ for m⁻². `densities()` gives the same for each family of Burgers vectors, with "other" last, as `lengths()`. A result read from a CA file has them too, from the box of its mesh.
- **Some atoms only.** In MDAPY this is the system of those atoms, `system.filter(mask)`; the other atoms are then as if they were not there, which is what OVITO's option to analyze the selected atoms does. The page shows the gallium of gallium arsenide analyzed as fcc, and how to spread the per-atom results back over every atom.

Tests:

- A quadrupole of straight lines: `density` is the number of lines times the box length over the volume, and the densities of the families add up to it.
- Every comparison with OVITO now also checks `density` against OVITO's `total_line_length / cell_volume`.
- A new test compares the gallium of a zinc blende screw quadrupole, analyzed as fcc, with OVITO's `only_selected=True` on the same selection. The selected atoms get the same structures atom by atom, OVITO makes every other atom "Other", and both find two 1/2<110> lines of the same length (within 1 %).
- The CA round trip keeps the densities.

All 3889 tests pass locally.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `655bc948bb8c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
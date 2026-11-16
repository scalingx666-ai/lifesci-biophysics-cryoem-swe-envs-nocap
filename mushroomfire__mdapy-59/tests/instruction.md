# Issue

**Title:** DislocationAnalysis: no longer experimental, name the fcc stair-rods

## Description

- The fcc families gain `1/6<310>` and `1/3<110>`: the stair-rods where Shockley partials on two {111} planes meet in the two ways other than the Lomer–Cottrell lock. They were `other`. A new test builds both locks and checks the families, the Burgers vectors of the rods and that the lines add up to the total Burgers vector in space. OVITO calls both `Other`, so the OVITO comparison sums them into `other`.
- The page drops the experimental box. It gains two sections:
  - **Limits**: grain boundaries, small loops, wide partials, disorder, units, order of atoms, turned lattices.
  - **Where the results differ from OVITO**: junctions, sliver clusters, grain boundaries, turned lattices, families, tessellation ties, lattice frames, options.
- The compatibility page no longer names experimental features.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `32dbfd0e6047`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
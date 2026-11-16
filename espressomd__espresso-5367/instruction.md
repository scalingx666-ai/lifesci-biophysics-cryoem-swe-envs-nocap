# Issue

**Title:** Add pair-bond precondition in GlueToSurface

## Description

GlueToSurface::initialize never validated the arity of bond_centers or
bond_vs, unlike the sibling protocols BindCenters and
BindAtPointOfCollision. A non-pair (e.g. angle) bond was accepted at set
time; at the first collision handle_collisions() stores it with a
single-partner list, so it gets misclassified as a pair bond and
calc_bond_pair_force throws BondUnknownTypeError inside a Kokkos parallel
region, leading to std::terminate far from the misconfiguration.

handle_collisions() inserts a single-partner list for both bond_centers
and bond_vs, so both must be strict pair bonds (exactly one partner).
Add the arity guards in initialize() at the same point the sibling
protocols do, raising a clear RuntimeError. Existence is already
guaranteed upstream by Protocol::find_bond_id, so at() is safe.

Extend test_glue_to_surface in collision_detection_interface.py with the
two assertRaisesRegex blocks mirroring the existing bind_centers and
bind_at_point_of_collision checks.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `79f7ae1cbca4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
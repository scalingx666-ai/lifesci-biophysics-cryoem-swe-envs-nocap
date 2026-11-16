# Issue

**Title:** Add support for chiral volume conversions

## Description

This PR adds support for converting chiral volumes from either `OFF -> ON` or from `ON -> OFF`. Note that it does not support *inversion* of chiral volumes. 

Consider the following transformation where we go from an sp2 center with a dummy atom attached to an sp3 center:

![image](https://github.com/user-attachments/assets/0a7f760d-3aba-4a99-8c24-ed7ac4c359d4)

If we break the core-dummy bond `(C-*)` at the left end-state, then the associated chiral volumes `[(C,O1,O2,*), (C,O1,H,*), (C,H,O2,*)]` will also need to be turned off, as well as any associated angle terms. So if we interpolate in a way such that the angles are turned on *before* the chiral volumes are turned on, eg. at `lambda=0.5`, we will have a mixture of chiral states, of which roughly half will be inconsistent with the chiral volume defined at the right end-state, leading to an HREX bottleneck. So instead, we need to turn on the chiral restraints *before* we turn on the angle terms. 

High-level implementation details:

1) Disables the chiral validity check both within the single topology code as well as the atom-mapping code (now raising a `DeprecationWarning`)
2) Refactors the logic for the interpolation of bonded parameters from one that determines directionality based on the force constant, to one that's based on inspecting the idxs themselves and assigning it into one of three types {core, dummy_a, dummy_b}. 
3) Explicitly identifies bonds/angles involved in chiral volume conversions that are being toggled depending on the type, and setting up an interpolation schedule that appropriate for the said type.
4) For terms that are not involved in chiral volume conversions, the old default schedule is used.
5) Adds three utility plotting functions `plot_core_interpolation_schedule`, `plot_dummy_a_interpolation_schedule`, and `plot_dummy_b_interpolation_schedule` for visualizing force constants associated with the transformation.

The flow chart for determining which schedule to use is as follows:

``` python
#
#                     bonded terms
#                      /       \
#                     /         \_____
#                    /                \
#                  core              dummy
#                 /   \              |    \
#          ______/     \             |     \_________________
#         /             |            |                       \
#        /              |            |                        \
#   converting    non-converting     A                         B
#    /      \                       / \                       / \
#   /        \                     /   \                     /   \
# on->off   off->on        converting non-converting converting  non-converting
#                           (on->off)                (off->on)
```

Lambda schedule implementing the above diagram.
``` python
CORE_BOND_MIN_MAX = [0.0, 1.0]
CORE_ANGLE_MIN_MAX = [0.0, 1.0]
CORE_TORSION_MIN_MAX = [0.0, 1.0]

# core terms that are involved in chiral volumes being turned on or off
CORE_CHIRAL_ATOM_CONVERTING_ON_MIN_MAX = [0.0, 0.5]
CORE_CHIRAL_ANGLE_CONVERTING_ON_MIN_MAX = [0.5, 1.0]
CORE_CHIRAL_ATOM_CONVERTING_OFF_MIN_MAX = _flip_min_max(CORE_CHIRAL_ATOM_CONVERTING_ON_MIN_MAX)
CORE_CHIRAL_ANGLE_CONVERTING_OFF_MIN_MAX = _flip_min_max(CORE_CHIRAL_ANGLE_CONVERTING_ON_MIN_MAX)

# non-converting (may be consistently in chirality or just achiral) dummy B groups that are turning on
DUMMY_B_BOND_MIN_MAX = [0.0, 0.7]
DUMMY_B_ANGLE_MIN_MAX = [0.0, 0.7]
DUMMY_A_BOND_MIN_MAX = _flip_min_max(DUMMY_B_BOND_MIN_MAX)
DUMMY_A_ANGLE_MIN_MAX = _flip_min_max(DUMMY_B_ANGLE_MIN_MAX)

# chiral and converting dummy B groups are turning on
DUMMY_B_CHIRAL_BOND_CONVERTING_ON_MIN_MAX = [0.0, 0.7]
DUMMY_B_CHIRAL_ATOM_CONVERTING_ON_MIN_MAX = [0.3, 0.5]
DUMMY_B_CHIRAL_ANGLE_CONVERTING_ON_MIN_MAX = [0.5, 0.7]  # angles are all turned off

# chiral and converting dummy A groups are turning off
DUMMY_A_CHIRAL_BOND_CONVERTING_OFF_MIN_MAX = _flip_min_max(DUMMY_B_CHIRAL_BOND_CONVERTING_ON_MIN_MAX)
DUMMY_A_CHIRAL_ATOM_CONVERTING_OFF_MIN_MAX = _flip_min_max(DUMMY_B_CHIRAL_ATOM_CONVERTING_ON_MIN_MAX)
DUMMY_A_CHIRAL_ANGLE_CONVERTING_OFF_MIN_MAX = _flip_min_max(DUMMY_B_CHIRAL_ANGLE_CONVERTING_ON_MIN_MAX)

# torsions are the same throughout
DUMMY_B_TORSION_MIN_MAX = [0.7, 1.0]  # chiral and achiral both use the same min/max for torsions
DUMMY_A_TORSION_MIN_MAX = _flip_min_max(DUMMY_B_TORSION_MIN_MAX)
```

Update [10/30/2024] - Support for visualizing interpolation schedule.

Pending a lot more systematic validation.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `344f11caf98e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
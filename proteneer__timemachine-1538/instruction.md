# Issue

**Title:** Aligned NonbondedInteractionGroup and NonbondedAllPairs potentials

## Description

This PR adapts the same alignment logic we used for bonded terms to the nonbonded all pairs and nonbonded interaction group potentials.

Motivation:

The `SingleTopology` class currently is responsible for three tasks:

1) Building non-alchemical end-states.
2) Building alchemical end-states. 
3) Interpolating between the non-alchemical end-states.

This PR is a step in moving 1) outside of the SingleTopology class as much as possible. Parameterizing of the `NonbondedInteractionGroup` in particular is very tricky since it:

1) Needs to invoke a custom ff handler specific to the ixn_group to parameterize the alchemical guest params (via `_get_guest_params()`, as well as re-using the same handler to get the non-alchemical host params. Ideally, the special parameterization logic of non-alchemical end-states should be completely *outside* of the `SingleTopology` class, eg. in `builders.py`. 
2) Currently, interpolation is done "in-line" - as part of the `combine_with_host` call. i.e. detracting from our standard "align and interpolate" paradigm.

Key Summary of changes:

1) `AtomMapMixin` no longer takes in (`mol_a`, `mol_b`) but rather their respective number of atoms. Rationale: reduce dependency on `Chem.Mol`, and only the `GetNumAtoms()` method is currently being used.
2) `SingleTopology::combine_with_host(host_sys, lamb)` now invokes the respective `_align_nonbonded_*` and `self._aligned_nonbonded_*.interpolate(lamb)` methods. This is currently sub-optimal since the re-alignment happens *each* time `combine_with_host(host_sys, lamb)` is called. In the near future, `SingleTopology` will instead directly be constructed from `GuestSystem/HostSystem/HostGuestSystem` objects so alignment of nonbonded terms can be done only once in the constructor.
3)  `AlignedPotential` classes are now moved to  `timemachine/fe/aligned_potential.py`

To Do:

Update Hashes

To Design with @mcwitt:

Whether or not we absorb the REST interpolation logic into the aligned potentials.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fac0c1da72a2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
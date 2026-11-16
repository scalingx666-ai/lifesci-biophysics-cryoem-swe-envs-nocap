# Issue

**Title:** Add option disallowing atom mappings with no chirally-valid dummy anchor assignment

## Description

## Background

1. Cancellation of dummy interaction contributions when computing $\Delta \Delta g$ relies on being able to factor dummy group interactions out of the partition function, which in turn requires that dummy groups be anchored to the core through at most one anchor atom. We implement this by disabling all core-dummy bonds except those involving a single core ("anchor") atom for each dummy group in the noninteracting state.
2. Numerical stability of MD requires that bond terms be turned on _before_ chiral volume terms (at least for the normalized version of the chiral volume that we currently use, which have a singularity when any of the 3 bonds shrinks to zero). Thus, when we disable a core-dummy bond in (1), we must also disable any chiral volume terms that span that bond.
3. We assert that every chiral center in the combined molecule be restrained (i.e. be associated with at least one chiral volume restraint term in the potential) at _both_ end states. This is to prevent kinetic trapping in an undesired chiral state, since our alchemical protocol turns on angle terms before chiral restraints.

Putting all this together, the present issue is that disabling bonds to maintain factorizability (1) necessitates disabling associated chiral volumes (2), which can result in a failure of the assertion (3).

## Approach

This PR seeks to work around the issue at the atom mapping level. Specifically, we add a new leaf filter (`enforce_chirally_valid_dummy_groups`) to the MCS search restricting the returned atom mappings to ones that have *at least one valid dummy group assignment* (i.e. not violating the chiral assertion (3)).

Separately, the end-state setup code in `SingleTopology` is updated to select a candidate dummy group assignment that does not violate (3), which is guaranteed to exist for atom mappings generated with the option `enforce_chirally_valid_dummy_groups=True`.

## Todo
- [x] handle X3 chiral centers
- [x] single topology test case for core-core bonds broken
- [x] consider renaming flag to something more accurate/less confusing (exposed `leaf_filter_fxn` instead)
- [x] randomize atom indices to strengthen tests

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `db56acb30158`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
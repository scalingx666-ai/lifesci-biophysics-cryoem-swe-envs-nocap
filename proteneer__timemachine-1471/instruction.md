# Issue

**Title:** Refactor VacuumSystem and HostGuestSystem

## Description

We currently have two polymorphic quirks we have in our poorly named `VacuumSystem` class:

1) The `VacuumSystem` dataclass is currently used by both *host* system and *guest* systems.
2)  `VacuumSystem` has a polymorphic attribute `nonbonded`, that can be used to represent both a nonbonded pairlist, as well as the nonbonded all pairs potential.

This PR:

1) Renames `VacuumSystem` -> `GuestSystem`, and its `nonbonded` attribute to `nonbonded_pair_list`
2) Introduces `HostSystem`, a container:
  - that is devoid of `chiral_atom` and `chiral_bond` attributes since the upstream deserializers do not generate them
  - that adds the `nonbonded_all_pairs` attribute.
3) Keeps the `HostGuestSystem` class, which has three nonbonded attributes: `nonbonded_pair_list`, `nonbonded_all_pairs`, `nonbonded_ixn_group`
4) Introduces an `AbstractSystem` class, defining the methods `get_U_fn()` and `get_U_fns()`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b3308905dc22`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
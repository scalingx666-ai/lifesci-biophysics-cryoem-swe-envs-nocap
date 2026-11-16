# Issue

**Title:** [BUGFIX] GCMC region keyword: graceful translation reject (#66883) + faster region+mol gas-list rebuild

## Description

This PR groups two related changes to `fix gcmc`, both around the `region` keyword
(same source file, `src/MC/fix_gcmc.cpp`).

**Summary**

1. **Graceful translation reject (forum #66883).** A region-restricted GCMC *translation*
   trial move that could not be placed inside the region within `max_region_attempts`
   (1000) tries aborted the entire simulation via `error->one()`, unlike the insertion
   paths which skip the trial move. All four translation routines now reject the
   unplaceable move and continue, emitting a one-time warning.
2. **Faster region+mol gas-list rebuild (issue #225).** `update_gas_atoms_list()` rebuilt
   the in-region gas list by looping over every molecule ID and calling `Group::xcm()`
   (an O(natoms) scan + an `MPI_Allreduce`) for each -- O(nmolecules*natoms) work and
   nmolecules collective calls per rebuild, run after every successful exchange/move. It
   now does a single pass over local atoms plus one `MPI_Allreduce`.

**Related Issue(s)**

- Forum report https://matsci.org/t/66883 (region-restricted moves appearing to hang).
-  (`fix gcmc` slowdown with `region` + `mol`).

**Author(s)**

Axel Kohlmeyer, Temple University.

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in
LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2)
or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

The code, the example input, and the documentation changes in this pull request were
generated with the assistance of Claude Code (Claude Opus 4.8). The MPI-collective-safety
design, the gas-list refactor, and the verification strategy were directed and reviewed by
the author. Consistent with the `Co-Authored-By` trailers on the commits.

**Backward Compatibility**

No input syntax changes. Behavior change for change (1): a region-restricted translation
that cannot be placed within 1000 attempts no longer aborts; it is rejected (still counted
as an attempt) with a one-time warning. Change (2) is behavior-preserving (see below).
Documented with `versionchanged:: TBD` in `doc/src/fix_gcmc.rst`.

**Implementation Notes**

*Translation reject* -- all four translation routines reject via a shared
`warn_region_reject()` helper, MPI-collective-safe: molecule variants (synchronized RNG +
Allreduce'd COM) `return` together; `attempt_atomic_translation` falls through to its
existing `MPI_Allreduce(&success)`; `attempt_atomic_translation_full` broadcasts the
owner-only failure with a one-int `MPI_Allreduce(MIN)` so all ranks skip `energy_full()`
together. The "outside box" checks (a genuine error) remain `error->one()`.

*Gas-list rebuild (#225)* -- the per-molecule `Group::xcm()` loop is replaced by one pass
accumulating each molecule's mass-weighted unwrapped position, one `MPI_Allreduce` over all
molecules, then normalize by the gas molecule mass and remap. Per rebuild this drops from
O(nmolecules*natoms) work + nmolecules collectives to O(natoms) work + 1 collective; the
collective-count reduction is the scalability win at high MPI rank counts. The refactor is
**bit-for-bit identical** to the previous algorithm: verified by building the old
`Group::xcm` path (with the indexing fix) and the new path side by side and confirming
identical trajectories for a region+mol CO2 GCMC run at 1 and 4 MPI ranks. It also fixes a
latent one-past-the-end read (COM arrays were sized `maxmol_all` but indexed by molecule ID
up to `maxmol_all`; now indexed into a `maxmol_all+1` buffer).

A new example `examples/mc/in.gcmc.region` (with reference logs for 1 and 4 ranks) showcases
the `region` keyword and documents a commented-out variant that triggers the change-(1)
warning.

**Post Submission Checklist**

- [x] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [x] Suitable new documentation files and/or updates to the existing docs are included
- [ ] The added/updated documentation is integrated and tested with the documentation build system
- [x] The feature has been verified to work with the conventional build system
- [x] The feature has been verified to work with the CMake based build system
- [x] One or more example input decks are included

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d71abe6102c4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
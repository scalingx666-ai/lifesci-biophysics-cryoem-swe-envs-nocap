# Issue

**Title:** KOKKOS: make extract_atom device-aware (sync to host before returning, #3945)

## Description

**Summary**

. When running with the KOKKOS package and per-atom data residing on an
accelerator device (e.g. a GPU), `lammps_extract_atom()` (and the Python/Fortran wrappers
built on it) returned a pointer to host data that could be **stale**. The host copy is only
guaranteed current at output steps and at the end of a run, but `extract_atom()` can be
called at arbitrary times -- e.g. from the LAMMPS GUI (which runs the simulation in a
separate thread) or from a `python` command during a run.

**Related Issue(s)**

.

**Author(s)**

Axel Kohlmeyer, Temple University.

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS
and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU
Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

The code and documentation changes in this pull request were generated with the assistance
of Claude Code (Claude Opus 4.8), following the approach outlined by @rbberger in #3945. The
design and verification were directed and reviewed by the author. Consistent with the
`Co-Authored-By` trailer on the commit.

**Backward Compatibility**

No API or input changes. Behavior change only under KOKKOS with a device backend:
`extract_atom()` now returns up-to-date host data instead of a possibly-stale copy.

**Implementation Notes**

`Atom::extract()` is made `virtual` and overridden in `AtomKokkos`. The override maps each
public property name to the KOKKOS data mask of the dual view that holds it, calls
`sync(Host, mask)` to refresh the host copy from the device, then delegates to
`Atom::extract()`. Custom per-atom double vectors/arrays (`d_*`/`d2_*`) sync via
`DVECTOR_MASK`. Names whose data is not device-resident are absent from the map and fall
through unchanged (their host copy is always current).

Notes:
- The atom-vec `sync()` only acts on masks for fields it actually owns, so passing a mask
  for an absent field is a safe no-op; and the `DualView` sync is a no-op when the host copy
  is already current.
- `atom->extract()` is called only from the library interface (`src/library.cpp`), so there
  is no impact on hot code paths.
- A doc note + `versionchanged:: TBD` was added to the `lammps_extract_atom()` docstring.

**Verification**

Built with the KOKKOS package (CMake). A smoke test through the Python module with
`-k on t 1 -sf kk` confirms `extract_atom()` for `x`, `v`, `id`, `type` returns correct
values with no regression and that the virtual override is reached (`lmp->atom` is an
`AtomKokkos`, created at `lammps.cpp:868`). The device->host staleness path itself cannot be
exercised on a CPU-only KOKKOS build (`LMPDeviceType == LMPHostType`, so dual views share
memory and the sync is a no-op); it requires a GPU build to observe.

**Post Submission Checklist**

- [x] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [x] Suitable new documentation files and/or updates to the existing docs are included
- [ ] The added/updated documentation is integrated and tested with the documentation build system
- [ ] The feature has been verified to work with the conventional build system
- [x] The feature has been verified to work with the CMake based build system

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7f680de2968e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Allow type labels with the KOKKOS package

## Description

**Summary**

Type labels (the `labelmap` command and the `* Type Labels` sections of data files) were refused whenever the KOKKOS package was active. Labels are resolved to numeric types on the host when commands and data files are parsed, so nothing about them depends on KOKKOS. This PR removes the refusal, syncs the host arrays in the two host-side label consumers that can run while the device holds the newer data, and restores the `labelmap check_labels` runtime check, which `ModifyKokkos::setup()` did not perform.

**Related Issue(s)**

None.

**Author(s)**

Jichen Li, Uppsala University, jichen.li@kemi.uu.se

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

AI tools (Cursor agent, Claude) were used to review the changes, simplify them, and prepare the commits and verification scripts. All changes were reviewed and tested by the author.

**Backward Compatibility**

No inputs are broken. Inputs that use type labels, which used to stop with "Label maps are currently not supported with Kokkos", now run with the KOKKOS package. Nothing changes for builds without KOKKOS.

**Implementation Notes**

- `Atom::add_label_map()`: removed the error for `lmp->kokkos`, and removed the matching notes in `labelmap.rst` and `read_data.rst`.
- `ModifyKokkos::setup()` replaces `Modify::setup()` but did not end with the type label check requested by `labelmap check_labels`, so the check was silently skipped with KOKKOS. The call is added.
- `LabelMap::check_labels()` runs from `Modify::setup()`, which comes before `VerletKokkos::setup()` syncs the host. It now calls `atom->sync_host_arrays()` for the tag, type and topology arrays first.
- `WriteData::write()` reads all per-atom arrays through the plain pointers, so it now calls `atom->sync_host_arrays(ALL_MASK)` first.
- Both syncs follow what `Set::invoke_actions()` and `CreateAtoms::command()` already do. Without KOKKOS, `sync_host_arrays()` does nothing.
- Tests: the existing label map tests in `test_labelmap`, `test_set_property`, `test_variables` and `test_molecule_file` are already registered with `add_kokkos_test()`, but skipped every case when a suffix was enabled. These skips are removed, so the same tests now also run with `-sf kk`. No new tests were added.

Verification:

- CMake build with KOKKOS/OpenMP (x86_64): the plain and Kokkos variants of Labelmap, SetProperty, Variables, MoleculeFile and Utils pass.
- CMake build with KOKKOS/CUDA (Hopper90) on an NVIDIA GH200: the plain and Kokkos variants of Labelmap, SetProperty, Variables and MoleculeFile pass, and every formerly skipped case runs on the GPU.
- Also on the GH200: `write_data ... types labels` output was compared between CPU, `-k on g 1 -sf kk` (newton off) and `-sf kk -pk kokkos neigh half newton on`. The comparison was made directly after `read_data`/`replicate`, and before and after runs with device-side atom sorting every step, and all outputs agree. `labelmap check_labels` reports the same results with KOKKOS as without. The energies and forces from a force field written with type labels match those from the same force field written with numeric types, on CPU and with KOKKOS.

**Post Submission Checklist**

- [x] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [x] Suitable new documentation files and/or updates to the existing docs are included
- [x] The added/updated documentation is integrated and tested with the documentation build system
- [ ] The feature has been verified to work with the conventional build system
- [x] The feature has been verified to work with the CMake based build system
- [x] Suitable tests have been added to the unittest tree. (existing tests enabled for KOKKOS)

**Further Information, Files, and Links**

None.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `885ad988b76a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
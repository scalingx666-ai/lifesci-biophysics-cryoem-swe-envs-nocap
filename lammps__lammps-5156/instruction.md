# Issue

**Title:** molecule-template member selection to fix gcmc

## Description

**Summary**

This PR adds a `molindex` keyword to `fix gcmc` that allows the user to select a specific molecule from a molecule template containing multiple molecule definitions.

This enables different `fix gcmc` instances to operate on different members of the same multi-molecule template, while using a common `fix rigid/small` integration fix.

If `molindex` is not specified, the first molecule in the template is selected, preserving the existing behavior.

**Related Issue(s)**

None.

**Author(s)**

Russell Kajouri, Institute for Computational Physics (ICP), University of Stuttgart.


**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Backward Compatibility**

This change is backward compatible.
If `molindex` is not specified, `fix gcmc` continues to use the first molecule in the selected molecule template, which matches the previous behavior.
Existing input scripts therefore do not need to be modified.

When using a molecule template with multiple molecules together with the `rigid` or `shake` keyword, the previous code called `set_molecule()` of fix rigid/small or fix shake for *every* molecule in the template, so that the inserted molecule received the rigid body or SHAKE properties of the *last* molecule in the template (e.g. wrong rigid body mass or constrained bond lengths). This is corrected by registering only the inserted molecule.

The follow-up changes (see below) also include a bugfix for using fix gcmc with fix rigid/small, which is part of a separate collection of small fixes as well: fix gcmc passed the center of mass instead of the geometric center of an inserted molecule to fix rigid/small, and did not update the cached geometric center of the molecule template after shifting its coordinates. Thus rigid molecules from molecule files whose coordinates are not centered on the origin (e.g. `examples/mc/H2O.txt`) were displaced by that offset (1.72 Angstrom for water) right after insertion. Inputs using `examples/mc/CO2.txt` were not affected, and the existing `in.gcmc.co2` and `in.gcmc.h2o` examples reproduce their reference logs exactly.

**Implementation Notes**

The implementation adds a new `molindex` keyword to `fix gcmc` 
for selecting one member of a molecule template that contains 
multiple molecule definitions.

Internally, `molindex` is specified as a 1-based index in the 
input script and converted to a 0-based index in the code. 
The selected molecule is then used consistently for molecular 
properties, insertion/deletion operations, and rigid-body registration.

When `fix gcmc` is used together with `fix rigid/small`, the rigid fix 
continues to use the original multi-molecule template. The selected `molindex` 
is passed to `FixRigidSmall::set_molecule()` so that the inserted molecule is associated 
with the correct member of that template. No changes to the RIGID package are required.

Correctness was verified using deterministic regression tests against an unmodified build of the same `upstream/develop` revision. For identical initial configurations, velocities, MPI settings, GCMC parameters, and random-number seeds:

- selecting ACN from a multi-molecule template reproduced  the original single-template ACN GCMC trajectory and insertion/deletion statistics exactly;
- selecting CO2 reproduced the corresponding original CO2 GCMC results exactly;
- reversing the molecule-template order and adjusting `molindex` accordingly produced identical GCMC statistics and particle-number trajectories.

The documentation and a small usage example have also been updated.
The added example file can be found `examples/mc/in.gcmc.molindex` with its attached logfile

Follow-up changes by the LAMMPS developers (pending merge of russellkajouri/lammps#1 into this branch, since maintainer edits are not possible for this pull request):

- merge of the rigid insertion bugfix described above (the identical commit is also in the collection of small fixes, so either pull request can be merged first without conflicts)
- addressed the automated review: removed commented-out code, use `utils::missing_cmd_args()` and point to the offending argument in error messages, fixed a typo and trailing whitespace, added `.. versionadded:: TBD`
- removed redundant molecule template size checks from `FixGCMC::init()`; more informative error and warning messages
- documentation: explain the purpose of `molindex` and that each fix gcmc command needs its own fix group, document the default and the same-template requirement for `rigid` and `shake`, replace an obsolete paragraph about multiple fix gcmc commands
- example: rank-independent initial velocities, no data or dump files written, removed the unused data file, reference logs `log.1Oct2026.gcmc.molindex.g++.1` and `.g++.4` following the naming conventions

**Artificial Intelligence (AI) Tools Usage**

OpenAI ChatGPT was used during the development of this contribution to discuss the implementation strategy, assist with drafting and reviewing modifications to `fix_gcmc`, help diagnose issues encountered during development, and assist with documentation(grammer modification and selecting words).

All source-code changes were manually reviewed by the author, compiled against the current LAMMPS `develop` branch, and independently validated using the regression tests described in this pull request.

The follow-up commits by the LAMMPS developers (russellkajouri/lammps#1) were prepared and tested with the help of Claude Code (Opus 5.5).

**Post Submission Checklist**

<!--Please check the fields below as they are completed **after** the pull request has been submitted. Delete lines that don't apply-->

- [ ] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [x] Suitable new documentation files and/or updates to the existing docs are included
- [x] The added/updated documentation is integrated and tested with the documentation build system
- [ ] The feature has been verified to work with the conventional build system
- [x] The feature has been verified to work with the CMake based build system
- [ ] Suitable tests have been added to the unittest tree.
- [x] One or more example input decks are included

**Further Information, Files, and Links**

A minimal example demonstrating the use of `molindex` with a multi-molecule
template is included in the `examples/mc` directory.
Additional validation details are provided in the Implementation Notes above.
and the files are accessible at https://github.com/russellkajouri/examples/tree/main/mixtures/CO2-ACN/

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a10dcbcf2569`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** src/INTEL/TEST: update compute_modify syntax for lc input files

## Description

**Summary**

Update the LC benchmark input files in `src/INTEL/TEST` to use the
current `compute_modify` syntax.

The following deprecated commands:

    compute_modify ... extra ...

were replaced with:

    compute_modify ... extra/dof ...

in:

- `in.intel.lc`
- `in.lc_generate_restart`

Without this change, the restart generation workflow fails with
current LAMMPS releases where the backward compatibility aliases
`extra` and `dynamic` were removed.

**Related Issue(s)**

None.

**Author(s)**

Evgeny Rybin, Intel

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

By submitting this pull request, I confirm that I did NOT use any AI tools to generate all or parts of the code and modifications in this pull request.

**Backward Compatibility**

No backward compatibility issues.

This change only updates deprecated input script syntax to the currently
supported `compute_modify ... extra/dof ...` form.

**Implementation Notes**

The input scripts `in.intel.lc` and `in.lc_generate_restart` still used
the deprecated `compute_modify ... extra ...` syntax.

Backward compatibility for the `extra` alias was removed in commit:

    3e4a50fe638db8b527948d50412cc230de41ce7b

("remove backward compatibility for compute_modify extra and
compute_modify dynamic")

The updated input files run correctly with current LAMMPS releases.

**Post Submission Checklist**

- [x] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] The source code follows the LAMMPS formatting guidelines

**Further Information, Files, and Links**

This is a maintenance fix for test/benchmark input files in
`src/INTEL/TEST`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c4fe7a5bcf91`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
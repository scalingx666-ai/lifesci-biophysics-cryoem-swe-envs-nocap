# Issue

**Title:** Updates, corrections, and enhancements of the documentation

## Description

**Summary**

This corrects multiple issues with the documentation and how the spell-checker works
and adds a Howto documenting how to create/use double-band EAM potentials using pair style hybrid/overlay.

**Related Issue(s)**



**Author(s)**

Axel Kohlmeyer, Temple U

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

The script to generated the double band EAM potentials was created by Claude Opus 4.8

**Backward Compatibility**

N/A

**Implementation Notes**

The following changes are included in this pull request

- restore some accidentally deleted notes about using Kokkos on Intel Xeon Phi / MIC / KNL
- restore the ability of 'make -C doc spelling' to check incrementally by using a separate doctree cache and pickle.
- Document how to simulate double-band EAM models in LAMMPS using pair style hybrid/overlay
 and implement script to generate suitable potential files for a specific example.

---

## These are notes from Claude Opus on the EAM howto:

Two-band ("double-band") EAM models -- e.g. the Fe-Cr potentials of Olsson et al.
(Phys. Rev. B 72, 214119 (2005)) and Bonny et al. (Phil. Mag. 91, 1724 (2011)),
requested in #648 -- add an extra "s-band" embedding term to the standard EAM
energy. Grouping the terms shows this is *exactly* a `hybrid/overlay` of two
`eam/fs` potentials, so it already runs with existing styles and needs no source
changes:

```
E_i = [1/2 sum_j V + F_d(rho_d)]  +  [F_s(rho_s)]
      \___ a standard eam/fs ___/     \__ eam/fs with V == 0 __/
```

The cross-only s-band density (zero for like pairs, nonzero only for unlike
pairs) requires the Finnis-Sinclair (`eam/fs`) form -- it cannot be expressed as
`eam/alloy`; the s-band file zeroes the pair (`r*phi`) section.

- `doc/src/Howto_eam_overlay.rst`: new Howto with the derivation, why the s-band
  must be `eam/fs`, the `pair_style hybrid/overlay` recipe, how to build the
  s-band file, and a `compute pair` energy-decomposition check. Linked from
  `Howto.rst` and `pair_eam.rst`.
- `tools/eam_generate/two_band_fecr.py`: writes the s-band `eam/fs` file
  (cross-only Slater density, `F_s = c1*sqrt(rho)+c2*rho^2+c3*rho^4`, zero pair);
  literature presets plus a self-contained, runnable `--demo` mode.

Verified by running the generated demo: the overlay reproduces
`E_total = E_dband + E_sband` exactly; a pure element has zero s-band energy; an
alloyed site has a nonzero, localized s-band term.

**Post Submission Checklist**

- [x] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [x] Suitable new documentation files and/or updates to the existing docs are included
- [x] The added/updated documentation is integrated and tested with the documentation build system

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f2f2286a153f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
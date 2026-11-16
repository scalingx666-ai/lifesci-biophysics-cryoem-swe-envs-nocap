# Issue

**Title:** GRANULAR: fix energy injection in fix wall/gran hooke/history at grazing impacts

## Description

**Summary**

`fix wall/gran hooke/history` could *gain* kinetic energy on a grazing oblique impact with friction (up to +34% at `mu=0.5`, ~76 deg from the wall normal), with the rebound tangential speed exceeding the incoming one. This one-line change corrects the underlying classic tangential model so the wall dissipates energy at all impact angles, matching the result of the modern `pair_style granular` (`linear_history`) model, the standalone `pair_style gran/hooke/history`, and the KOKKOS `fix wall/gran/kk`.

**Related Issue(s)**

None. Found while developing a YAML-driven granular/DEM regression test suite.

**Author(s)**

Axel Kohlmeyer (LAMMPS Development Team)

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

This change was developed with the assistance of Claude Code (Anthropic, Claude Opus). The AI tool was used to investigate and characterize the energy-injection behavior and to draft the code change, the explanatory comment, and the documentation note; the diagnosis and the changes were reviewed by a human. This is consistent with the `Co-Authored-By` trailer on the commits.

**Backward Compatibility**

Yes. This changes the *frictional* behavior of `fix wall/gran hooke/history`. Previously the classic tangential model (`GranSubModTangentialLinearHistoryClassic`) set `contact_radius_flag = 1`, scaling the tangential elastic force by the overlap-dependent contact radius `sqrt(delta*R)`. That makes the effective tangential stiffness both much smaller than the nominal `kt` and time-varying within a contact (non-conservative). After this change the tangential stiffness is the constant `kt`, as in the original standalone `pair_style gran/hooke/history` and the modern `linear_history` model. Simulations using `fix wall/gran hooke/history` with non-zero friction will therefore produce different (corrected) trajectories. The frictionless case, `fix wall/gran hertz/history`, and the standalone `pair_style gran/*` styles are unaffected. A `.. versionchanged:: TBD` note documenting this is included in `doc/src/fix_wall_gran.rst`.

**Implementation Notes**

Root cause: `define_classic_model` maps the `hooke/history` keyword to a Hooke normal model plus the `GranSubModTangentialLinearHistoryClassic` tangential model, whose constructor set `contact_radius_flag = 1`. For a Hooke contact `contact_radius = sqrt(delta*R)` varies during the contact, so the tangential elastic force `-kt * contact_radius * history` has a time-varying stiffness. This is non-conservative and pumps kinetic energy when the contact normal rotates quickly within a single contact, i.e. on grazing oblique impacts. The original `pair_style gran/hooke/history` and the modern `linear_history` both use a constant `kt`; only the Hertzian variant (`mindlin_classic`, used by `hertz/history`) should scale the tangential stiffness by the contact radius, and it sets the flag in its own constructor.

Fix: set `contact_radius_flag = 0` in the `GranSubModTangentialLinearHistoryClassic` constructor.

KOKKOS: the KOKKOS variant `fix wall/gran/kk` does **not** need this change. It derives from the internal `WALL/GRAN/OLD` class (`src/KOKKOS/fix_wall_gran_old`), and its device kernel computes the tangential force as `-(kt*history + meff*gammat*vtr)` with no contact-radius factor, so it already uses the constant-stiffness (conservative) form and only supports `hooke/history`. This change brings the modern CPU `fix wall/gran` into agreement with the KOKKOS path.

Verification (single sphere onto a `zplane` wall, no gravity, ratio of post- to pre-collision total kinetic+rotational energy; converged with decreasing timestep):

| case | before | after |
| --- | --- | --- |
| hooke/history, ~76 deg, mu=0.5 | 1.342 (energy gain) | 0.727 (matches `linear_history`) |
| hooke/history, mu=0.5, full angle sweep | up to 1.34 | 0.73 - 0.85 (always < 1) |
| hooke/history, frictionless | 0.988 | 0.988 (unchanged) |
| hertz/history, ~76 deg, mu=0.5 | 0.735 | 0.735 (unchanged) |

No existing force-style regression YAML uses this model.

**Post Submission Checklist**

- [x] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [x] Suitable new documentation files and/or updates to the existing docs are included
- [x] The feature has been verified to work with the CMake based build system
- [ ] Suitable tests have been added to the unittest tree.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7f680de2968e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
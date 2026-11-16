# Issue

**Title:** HIPPO: use the Pauli repulsion convention of current Tinker PRM files

## Description

**Summary**

HIPPO in LAMMPS uses the Pauli repulsion convention that Tinker had before version 8.9, so with any current Tinker HIPPO parameter file the repulsion is 64 times too large. This PR switches to the current convention (and converts the example file, whose results do not change), warns about files in the old convention, accepts the `d-equals-p` keyword of current files, and stops with an error for charge flux, which is not supported and was silently skipped.

**Related Issue(s)**

None found. Found while validating LAMMPS AMOEBA/HIPPO against Tinker ( and #5257).

**Author(s)**

Paul Saxe (MolSSI, Virginia Tech), with Claude (Anthropic)

**Licensing**

By submitting this pull request, I agree that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Backward Compatibility**

PRM files in the old convention need their repulsion sizes multiplied by 8. LAMMPS now warns when a file looks like one of them. The only such file in the distribution, `examples/amoeba/hippo_water.prm`, is converted here, and the HIPPO examples give thermo output identical to their reference logs. A PRM file with charge flux records now stops with an error instead of being run without them.

**Implementation Notes**

Tinker commit e6c9585 (2021-01-05, "change prefactor for HIPPO repulsion damping terms", first released in Tinker 8.9) changed the overlap prefactors in `damprep()` from 128 to 2 (equal damping exponents) and from 8192 to 128 (unequal ones). The same commit multiplied every repulsion size in Tinker's HIPPO parameter files by 8, so Tinker's energies did not change. For example, `water19.prm` went from 2.8758/2.0610 to 23.0064/16.4880. LAMMPS still has the old prefactors, in `PairAmoeba::damprep()` and in `lib/gpu/lal_hippo_extra.h`, and the old sizes in `examples/amoeba/hippo_water.prm`.

The PRM files carry no marker of the convention; Tinker's change touched only the repulsion records. Over all 106 records Tinker converted, the old sizes are 1.18 to 5.09 and the new ones 9.45 to 40.73. The new warning is printed when the largest size in a file is below 7.

Changes:

- `damprep()` (CPU) and its GPU version use the current prefactors.
- `examples/amoeba/hippo_water.prm`: all repulsion sizes multiplied by 8, including the commented-out alternatives.
- PRM reader:
  - warns about old-convention sizes;
  - accepts `d-equals-p` for HIPPO (whose direct field is always scaled like the polar field) and rejects it for AMOEBA;
  - stops with an error at charge flux records (`bndcflux`, `angcflux`) instead of skipping the "Charge Flux Parameters" section with only a log message.
- `doc/src/pair_amoeba.rst`: a paragraph on the convention and on charge flux.
- New `unittest/commands/test_pair_hippo.cpp` (`ctest -R PairHippo`), on the HIPPO water hexamer:
  - repulsion and nonbonded energy against Tinker (94.7840 and -47.3622 kcal/mol);
  - forces against finite differences;
  - the warning for old-convention sizes;
  - the charge flux error;
  - `d-equals-p` accepted for HIPPO and rejected for AMOEBA.

  Without the fix the energy, warning, charge flux and `d-equals-p` tests fail.

Validation against Tinker 8 (version 26.2), energies in kcal/mol:

| system, parameters | term | LAMMPS before | LAMMPS after | Tinker |
|---|---|---|---|---|
| water hexamer, Tinker's current `water21.prm` (HIPPO-WATER-2021), charge flux removed | repulsion | 6010.1120 | 93.9080 | 93.9080 |
| same | total nonbonded | 5869.0451 | -47.1589 | -47.1590 |
| 216-water box (`examples/amoeba`), `water21.prm` without charge flux | repulsion | 258885.1658 | 4045.0807 | 4045.0807 |
| same | multipoles / polarization | | -3486.7248 / -768.2396 | -3486.7248 / -768.2396 |
| water hexamer, converted `examples/amoeba/hippo_water.prm` | total potential | -43.7166 (old file) | -43.7166 | -43.7166 |
| 216-water box, converted example file | total potential | -1944.2893 (old file) | -1944.2893 | -1944.2893 |

For "before" the `d-equals-p` line was also removed from `water21.prm`, because the old reader rejects it. Tinker was run on the same files, with charge flux switched off (`chgflxterm none`). Dispersion and charge transfer were already correct. `hippo/gpu` (A100, mixed precision) gives -1944.3375 for the converted water box and -1932.6359 for the `water21.prm` box (CPU: -1944.2893 and -1932.5836). All six `examples/amoeba` water inputs (AMOEBA and HIPPO) give thermo output identical to the reference logs. The existing `AngleStyle:amoeba` test still passes.

Not included: support for charge flux. It makes the monopoles functions of bond lengths and angles, adding force and virial terms through the multipole, polarization and PME code on CPU and GPU. That is a feature of its own.

Review: both Copilot suggestions were taken in 69e964b, the `.. versionchanged:: TBD` directive in `doc/src/pair_amoeba.rst` and the removal of a trailing space in `unittest/commands/hippo_water.prm`.

**Post Submission Checklist**

- [x] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [x] Suitable new documentation files and/or updates to the existing docs are included
- [x] The added/updated code has been integrated into the unit test system
- [ ] A package specific README file has been included or updated (not needed)
- [x] One or more example input decks are included (the converted `examples/amoeba/hippo_water.prm`)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a0e2bd6efb3d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
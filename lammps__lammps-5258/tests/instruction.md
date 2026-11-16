# Issue

**Title:** AMOEBA/HIPPO: fix polarization forces and virial with equal Ewald coefficients, and the induced dipole predictors

## Description

**Summary**

AMOEBA and HIPPO polarization forces and virial are wrong in periodic (PME) runs whenever `pewald-alpha` equals `ewald-alpha`. That includes the default: both alphas default to the same value when neither keyword is given. The polarization energy is correct, but the forces are not its gradient, and the pressure is biased. This PR fixes it by giving `udirect1()` its own work arrays, as Tinker does.

It also fixes the induced dipole predictors (`polar-predict`). The Gear and ASPC coefficients were never set, so the predicted dipoles came from uninitialized memory, which could stop runs with a misleading "hal cannot find H bond partner" error. The least-squares predictor gave results that depended on the number of MPI ranks.

**Related Issue(s)**

None found. The closest are #3385 and #3386, which concern dipole convergence.

**Author(s)**

Paul Saxe (MolSSI, Virginia Tech), paul@thesaxes.net

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

Claude Code (Opus 5.5) was used to locate and fix the bugs in this pull request, to write the unit tests and the example in `examples/amoeba`, and to run the validation against finite differences and Tinker.

**Backward Compatibility**

Results change only for periodic AMOEBA/HIPPO runs with equal Ewald coefficients for multipoles and polarization, and those results were wrong. Runs with different coefficients, such as the other inputs in `examples/amoeba`, and non-periodic runs give identical results.

With `polar-predict gear` or `polar-predict aspc` the predicted dipoles now follow Tinker; before, they came from uninitialized memory. With `polar-predict lsqr` the results no longer depend on the number of MPI ranks. Runs without `polar-predict` are unchanged.

**Implementation Notes**

When `aewald == aeewald`, `PairAmoeba::polar_kspace()` (and `PairAmoebaGPU::polar_kspace()`) skips its own PME solve for the permanent multipoles. It re-uses the `fphi` and `cphi` member arrays that `multipole_kspace()` filled, which are scaled by `electric/am_dielectric`. But `induce()` runs between the two, and its `udirect1()` writes its unscaled potential into the same member arrays. `polar_kspace()` therefore picks up a potential that is about 332 times too small. As a result:

- the reciprocal-space force between the induced dipoles and the permanent multipoles (the `fphi` term) is effectively missing;
- the matching virial terms (the `cphi` term) are effectively missing.

Tinker's `udirect1()` carries a comment about exactly this: "note that cmp, fmp, cphi and fphi should not be made global since corresponding values in empole and epolar are different". The LAMMPS port made them class members. With this patch `udirect1()` writes to new members `fphidir`/`cphidir`; `cmp`/`fmp` get the same values in both routines, so they are unchanged. The GPU package's `induce()` calls the CPU `udirect1()` through `dfield0c()`, so the same change fixes `amoeba/gpu` and `hippo/gpu`.

Validation, comparing analytic forces to central finite differences of the total energy (h = 1e-4 Å, polar-eps 1e-10). Max |F - F_FD| in kcal/mol/Å:

| system | before | after |
|---|---|---|
| AMOEBA water hexamer in a 10 Å periodic box, only `ewald` (alphas 0.6386/0.6386) | 1.165 | 1.2e-5 |
| `examples/amoeba` water box (AMOEBA), `pewald-alpha 0.4` = `ewald-alpha` | 0.164 | 1.2e-5 |
| `examples/amoeba` water box (HIPPO), `pewald-alpha 0.4` = `ewald-alpha` | 0.093 | 1.2e-5 |
| 32 ethylene carbonate molecules (Poltype2 AMOEBA, 320 atoms), alphas 0.45/0.45 | 1.006 | 1e-5 (*) |

(*) Excluding two atom pairs that sit at 7.0000 Å, exactly on the real-space cutoff, where the energy steps.

For the ethylene carbonate cell, LAMMPS against Tinker 8 (`testgrad`/`analyze`), with matched PME settings:

- rms of the difference in per-molecule net force: 31.8 meV/Å before, 0.63 meV/Å after (Tinker's own rms net force is 259 meV/Å). What remains equals the permanent-multipole-only difference between the two codes.
- internal virial: up to 17 kcal/mol different before (about 800 atm), 0.02 kcal/mol after.

All six `examples/amoeba` water inputs (AMOEBA and HIPPO; box, hexamer, dimer) give thermo output identical to the reference logs.

A new unit test, `unittest/commands/test_pair_amoeba.cpp` (`ctest -R PairAmoeba`), puts the AMOEBA water hexamer from `examples/amoeba` in a 10 Å periodic box. It runs with equal and with different Ewald coefficients and checks the forces against central finite differences of the energy, and the diagonal of the pair virial against the derivative of the energy with respect to a uniform strain. Without the fix the two equal-coefficient tests fail (force error 1.16 kcal/mol/Å, virial error 1.94 kcal/mol); with it all four pass. The force-style YAML harness can't be used for `pair_style amoeba`, because the harness computes per-atom energies, which AMOEBA does not support. The existing `AngleStyle:amoeba` test still passes.

The GPU package reaches the patched `udirect1()` through the CPU `dfield0c()`, and `PairAmoebaGPU::polar_kspace()` has the same re-use branch, so `amoeba/gpu` had the same bug and gets the same fix. On an A100 (CUDA 12.2, mixed precision, built from this branch), the ethylene carbonate cell gives a per-molecule net-force difference from Tinker of 0.63 meV/Å, the same as the CPU build. The unfixed GPU build gives 31.8 meV/Å.

Induced dipole predictors (added later, commits "set the gear and aspc predictor coefficients in amoeba" and "sum the lsqr predictor normal equations over all mpi ranks in amoeba"):

- `allocate_smallsize()` allocated the `gear` and `aspc` coefficient arrays but never filled them, and `ulspred()` read them once the history of `maxualt` dipole sets was full. They now get the values of Tinker's `predict.f`: the 6th-order Gear binomial coefficients and the 16 ASPC coefficients of Kolafa. In an 8640-atom ethylene carbonate box on 4 or more MPI ranks, `polar-predict aspc` made the dipole iterations fail to converge after step 17 and the run stopped with "AMOEBA hal cannot find H bond partner - ghost comm is too short"; on 1 and 2 ranks the same input happened to run, on 1 rank no faster than without a predictor.
- For `polar-predict lsqr`, `ulspred()` summed the normal equations over the atoms each rank owns, so the ranks solved for different coefficients. The sums are now reduced over all ranks with `MPI_Allreduce`, and the full matrices are zeroed first, since the reduction includes the unused lower triangle.
- Two new tests in `test_pair_amoeba.cpp`, `predictor_gear` and `predictor_aspc`: at a fixed geometry the predictor history fills with the converged dipoles; then, with `polar-iter 1`, a single iteration from the predicted dipoles must give the converged energy. Without the fix the error is 0.92 kcal/mol; one iteration from the direct field (no predictor) is off by 0.04 kcal/mol. The least-squares predictor cannot be tested this way, since identical history vectors make its normal equations singular (it gives NaN with and without this change).
- In the 8640-atom box (50 steps, polar-eps 1e-5), the predictors now run without convergence warnings (ASPC on 1, 2 and 4 ranks, Gear on 4, LSQR on 1 and 4) and match the run without a predictor to 0.003 kcal/mol in the energy after 50 steps. ASPC gives identical thermo output on 1, 2 and 4 ranks, and LSQR on 1 and 4. On 1 rank the loop time drops from 56.8 s to 47.7 s (ASPC) and 44.3 s (LSQR).

While testing `amoeba/gpu` I found that every run exits with a segfault in `fftw_destroy_plan()`. That is a separate problem in `KSPACE/fft3d.cpp`, with its own pull request (#5257).

**Post Submission Checklist**

- [x] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] AI tools attribution is included
- [x] The source code follows the LAMMPS formatting guidelines
- [x] Suitable new documentation files and/or updates to the existing docs are included (none needed)
- [x] The added/updated code has been integrated into the unit test system
- [x] A package specific README file has been included or updated (none needed)
- [x] One or more example input decks are included (`examples/amoeba/in.water_box_equal_alpha.amoeba` with logs for 1 and 4 MPI ranks, and the unit-test input `unittest/commands/data.amoeba_water` with a copy of `amoeba_water.prm`)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c18cb87604a1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
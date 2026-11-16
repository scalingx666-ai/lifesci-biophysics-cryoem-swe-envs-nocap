# Issue

**Title:** Adding bias potentials and cv functions

## Description

Added functionality to run biased simulations. 

A CV function and a bias potential are individually defined and combined in the struct BiasPotential, which is passed to `AtomsCalculators.potential_energy` and `AtomsCalculators.forces!` as a general interaction type. 

Most code was added to `src/bias`, which contains the two files `bias.jl` and `cv.jl`. In `bias.jl`, methods to calculate the potential energy and forces that result from the added bias are defined, while `cv.jl` contains methods to calculate values of a number of CVs (distance, RMSD, Rg). The force calculation is split into calculating the derivative of the bias potential with respect to the CV and calculating the gradient of the CV with respect to the coordinates. The latter is by default done with autodiff, and `cv_gradient` is hence defined in `MollyEnzymeExt.jl`.

The function `kabsch` was added to `analysis.jl`. `kabsch` is needed in the calculate_cv method for the RMSD CV. 

All related tests are in `test/bias.jl` and new documentation under  `Biased simulations` in `documentation.md`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fc5aa1c4aa1e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
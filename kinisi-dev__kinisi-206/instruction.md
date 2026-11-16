# Issue

**Title:** Refactor YehHummer to use linear (D_0, slope) parameterization

## Description

Reparameterize YehHummer fitting from (D_0, viscosity) to (D_0, slope) where the model is linear: D_PBC = D_0 - slope/L. This improves MCMC sampling efficiency since viscosity spans many orders of magnitude while slope is better behaved in parameter space.

Changes to yeh_hummer.py:
  - Fit slope directly instead of viscosity
  - Convert viscosity bounds to slope bounds (inverted due to inverse relationship)
  - Simplify _yeh_hummer_function to inline the linear formula
  - Remove redundant yeh_hummer_linear static method and bounds_values
  - Add shear_viscosity property that converts slope samples to viscosity
  - Add return type annotation (sc.Variable | Samples)

Changes to fitting.py:
  - Add validation in log_likelihood for invalid model values
  - Return `-inf` for non-finite or non-positive model outputs
  - Prevents MCMC from accepting unphysical parameter combinations
    (e.g., VTF when T0 >= T)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a87ee353921f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
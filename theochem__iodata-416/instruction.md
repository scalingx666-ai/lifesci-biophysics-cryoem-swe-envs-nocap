# Issue

**Title:** Add decontract to convert a contracted basis to primitives

## Description

This adds `decontract` to `iodata/convert.py`. It takes a `MolecularBasis` and returns a basis where every shell has one primitive and one contraction, plus a sparse matrix that moves arrays to the new basis. It follows the "return the transformation matrix" option from the issue and has the same shape as `convert_to_cartesian` in #412.

- Primitives are identified by center, angular momentum, kind and exponent. If the same primitive shows up more than once (generalized contractions, or a shell that reuses an exponent from another shell), it is included once. Otherwise the new basis would be linearly dependent.
- The matrix has shape `(nbasis_new, nbasis_old)`. Coefficients go to `transform @ coeffs`, a density matrix to `transform @ dm @ transform.T`, and an overlap matrix as `transform.T @ overlap_new @ transform == overlap_old`.
- Pure and Cartesian shells both work. The conventions and primitive normalization are copied from the old basis.

Tests:
- a small hand made basis (shared exponent, SP shell, generalized s shell, pure and Cartesian d), checking shells and matrix entries
- function values on random points before and after, for a Cartesian basis
- `o2_cc_pvtz_pure.fchk`, `o2_cc_pvtz_cart.fchk` and `water_ccpvdz_pure_hf_g03.fchk`: the old overlap matrix is recovered from the new one with `compute_overlap`, and the orbitals keep the same overlap
- an empty basis

I only did the basis and the matrix. Converting a whole `IOData` object in one call could be a follow up once the `prepare` discussion in #191 settles.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0343d0d81c12`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
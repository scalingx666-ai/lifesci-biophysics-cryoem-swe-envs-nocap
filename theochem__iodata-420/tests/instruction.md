# Issue

**Title:** Compute the overlap integrals of all primitive pairs of a shell pair at once

## Description

Loading `psi4_cuh_cc_pvqz_pure.molden` (75 KB, one atom) took about 11 s on my machine, and `psi4_zn_cc_pvqz_pure.molden` about 10 s. The Molden loader checks the normalization of the orbitals with `compute_overlap`, and it does that up to four times per file. A profile of the load shows that almost all of the time is in `GaussianOverlap.compute_overlap_gaussian_1d`, which was called about 4 million times from Python, once for every pair of primitives, every pair of Cartesian functions and every direction.

This calls the one-dimensional integral once for all primitive pairs and Cartesian functions of a shell pair, with NumPy broadcasting:

- `compute_overlap_gaussian_1d` accepts arrays that broadcast to each other. It still gives the same numbers for scalars. The binomial coefficients are a lookup table, and the terms with `i > n1` or `j > n2` drop out because their coefficient is zero.
- In `compute_overlap` the loops over primitives are gone, the `np.frompyfunc` call is gone, and the powers of x, y and z for each angular momentum are made once and not again for every shell pair. The prefactor cut-off for negligible primitive pairs (`1e-15`) works the same way as before.

The two molden files above now load in 0.4 s each.

I compared the new `compute_overlap` with the old one on the 69 test files with an L2 basis (fchk, wfx, wfn and mwfn). The largest difference is 2.2e-16. The time for all 69 together went from 3.2 s to 2.7 s. Bases with thousands of shells of one primitive each (the decontracted wfn and wfx files, for example `cah110_hf_sto3g_g09.wfx`) take about the same or a bit longer than before, because there the cost is the NumPy calls per shell pair, not the number of primitives.

I added `test_gaussian_overlap_1d_broadcast`, which checks the broadcast version against a plain double loop for random positions, exponents and angular momenta up to 5. The existing overlap tests with reference numbers, and the molden, orca and the other tests, pass (547 tests in the whole suite before the new test was added).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `10c71d590918`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
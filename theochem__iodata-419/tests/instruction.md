# Issue

**Title:** Load the overlap and density matrices from MWFN files

## Description

Part of #232

The MWFN loader stopped after the orbitals and ignored the `# Various matrices` sections that Multiwfn writes. The three MWFN test files all have them, so this loads them:

- `$Overlap matrix` goes to `one_ints["olp"]`
- `$Alpha density matrix` and `$Beta density matrix` give `one_rdms["scf"]` (the sum) and `one_rdms["scf_spin"]` (the difference)
- `$Total density matrix` goes to `one_rdms["scf"]` when there is no alpha and beta pair

Each section starts with `$name, dim=  n  m  lower= k`. With `lower= 1` only the lower triangle is stored, one row after the other, and it is expanded to the full symmetric matrix. A section with `lower= 0` is read as a full matrix. The matrices are in the same basis function order as the orbital coefficients, so nothing needs to be converted.

The sections are found by their header line and are not expected in a fixed order, and sections with other names are skipped. That covers the second part of #232 for the sections after the orbitals. The sections before them are still read in the order that Multiwfn writes them.

Tests, using the three files in `iodata/test/data`:
- the overlap matrix from the file is the same as `compute_overlap` of the basis set (CH3 HF, CH3 ROHF and He with s to h functions)
- the trace of density times overlap is the number of electrons (5 alpha and 4 beta for CH3, 2 for He), and the trace of the spin density times the overlap is `spinpol`
- the first values of the lower triangle in the file end up in the right place, and the alpha density matrix is the same as the one built from the occupied orbitals
- the file with only a total density matrix has no `scf_spin`

The new tests fail without the change, because the loader returned no matrices. The whole test suite passes.

Not done: other sections that Multiwfn can write (for example Fock or kinetic energy matrices) are skipped. I did not have a file with them to check the names, and I did not want to guess.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c44bf8f4a9af`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
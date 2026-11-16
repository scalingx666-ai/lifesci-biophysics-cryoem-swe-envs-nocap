# Issue

**Title:** Pace omp feature

## Description

**Summary**

Add shared-memory version of pair style pace to the OPENMP package.

**Author(s)**

Lorenzo Piersante
lorenzo.piersante@epfl.ch

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

I used GPT-5.1 to analyze the OPENMP and ML-PACE codebases to suggest a list of changes needed to make pair_style.cpp OMP-parallel. While the main suggestions were right, substantial human work was required to make the code thread-safe. This mostly entailed careful scrutiny of the ACE codebase used by ML-PACE.

**Backward Compatibility**

No known issues.

**Implementation Notes**

The code mainly consists of several edits to PairPACE::compute to make it OMP parallel. The implementation mirrors the key OPENMP features of pair_meam_spline_omp.cpp to achieve OMP parallelization. These are:
- insertion of the OMP pragma before the force tally loop;
- use of the `loop_setup_thr` and `ev_setup_thr` functions to break up the `for` loop;
- use of threaded functions available in the OPENMP package to tally energy and force contributions.

Thread safety was ensured by creating a separate `ACERecursiveEvaluator` and `ACECTildeBasisSet` objects for each thread. They are stored in a vector with the size of the number of threads.

Correctness was verified by comparing the energies and forces of two LAMMPS NPT runs: one using the standard MPI pace implementation and one using hybrid MPI-OMP parallelism. Results were identical up to numerical precision (10^-11 for the norms of the forces).

The shared-memory version is as efficient as the MPI implementation below 16 threads, and then its efficiency deteriorates somewhat. Testing the hybrid execution on the HPC cluster of my university showed that for certain rank x threads configurations it was possible to achieve speed-ups of the order of 10% relative to full MPI simulations.

Other features in LAMMPS are not affected.

**Post Submission Checklist**

- [x] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] AI tools attribution is included
- [x] The source code follows the LAMMPS formatting guidelines
- [x] Suitable new documentation files and/or updates to the existing docs are included
- [x] The added/updated documentation is integrated and tested with the documentation build system
- [x] The feature has been verified to work with the conventional build system
- [x] The feature has been verified to work with the CMake based build system
- [x] Suitable tests have been added to the unittest tree.

**Further Information, Files, and Links**

Link to arXiv preprint making use of the code will be provided.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `39dc004630af`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
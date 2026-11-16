# Issue

**Title:** TTMETAD: make WALKERS_MPI work with more than one rank per walker

## Description

##### Description

.

 for `TTMETAD` with `WALKERS_MPI`. `multi_sim_comm` is valid only on the first rank of each replica, but `TTMETAD` read the walker count and index from it, and communicated over it, on every rank. With more than one MPI rank per walker (domain decomposition), the other ranks of each walker believed they were walker 0 of 1, and the run hung.

- Walker count and index come from `getNumberOfReplicas()` / `getReplicaIndex()` (#1481).
- The walkers directory, the sketch count and the gathered hills go over `multi_sim_comm` on the first rank of each walker only, then are broadcast over `comm`.
- The sketch seed is broadcast over `comm`. Without `DETERMINISTIC` it came from `time(nullptr)` on each rank, so the ranks of one walker could build different sketches.
- Only one rank of the replica writes the sketch file, instead of all of them writing the same HDF5 file.

Single-walker runs and runs with one rank per walker behave as before: the existing `ttsketch/rt-ttmetad` and `rt-ttmetad-2` pass unchanged.

##### Target release

I would like my code to appear in release __master__ (v2.11)

##### Type of contribution

- [ ] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [X] changes to a module not authored by you
- [ ] new module contribution or edit of a module authored by you

##### Copyright

- [X] I agree to transfer the copyright of the code I have written to the PLUMED developers or to the author of the code I am modifying.

##### Tests

- [X] I added a new regtest or modified an existing regtest to validate my changes.
- [ ] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

New `ttsketch/rt-ttmetad-walkers-dd`: 2 walkers with 2 ranks each under `driver --debug-dd yes`. Walker 1 reads the same frames in reverse order, and two sketches are built. `COLVAR`, `HILLS` and all four force dumps are byte-identical to the run with one rank per walker. Without the fix the run hangs. Tested locally with ITensor 3.2.0 built as in `.ci/install.itensor`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6bc5734a3d82`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
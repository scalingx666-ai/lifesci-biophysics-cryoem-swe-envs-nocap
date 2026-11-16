# Issue

**Title:** MPI: matrices used only row by row are not summed over the processes

## Description

The matrix actions of a chain like CONTACT_MATRIX -> SPHERICAL_HARMONIC -> MATRIX_VECTOR_PRODUCT (Q6, COORDINATIONNUMBER, LOCAL_AVERAGE, ...) compute one row per task, and the tasks were shared between the MPI processes one every stride. After the loop, every process summed the whole value stash (forward) and the whole force buffer (backward) over all the processes: for Q6 on 20000 atoms these sums are about 26 x 20000 x (neighbours) doubles per step and per action, and the calculation got slower with more processes (on one 32-core node: 1.7 s per step with one process, 4.2 s with 32).

Now a link producer -> consumer is local when the producer computes its rows independently (AdjacencyMatrixBase, FunctionOfMatrix: canDistributeRows) and the consumer only reads, and writes forces on, the row of the argument that its task works on (FunctionOfMatrix, MatrixTimesVectorBase: readsOnlyOwnRowOfArgument). ActionWithVector::getRowDistribution works this out once from the users of the action. The actions with a local link share their tasks by row (row r on process r%size); a producer whose users are all local links does not sum its values over the processes, and copies only its rows into the Values; a consumer does not sum the forces on the arguments of local links (sumForcesOverProcesses). Any other user (SUM, PRINT, TRANSPOSE, a mask, SERIAL, the GPU actions) keeps the sums, so that the behaviour is the same as before. MatrixTimesVectorBase keeps active the rows of a local argument owned by the other processes: whether they are active depends on values that are not known on this process.

New regtest rt-matrix-rows-mpi (3 processes: Q6, LOCAL_AVERAGE and a SUM of the contact matrix, same results and forces as with one process). All the driver regtests of symfunc, adjmat, matrixtools, clusters, function, crystdistrib, volumes, contour, gridtools and xsprint were also run with 2 and 3 processes: they give the same results as with one, except those that use AVOID_THREAD_GATHER, which is refused with MPI as before.

Claude-Session: https://claude.ai/code/session_016vwxtVAm1zaQVf1Tk5nVGy

<!--
  Feel free to delete not relevant sections below
-->

##### Description

<!-- describe here your pull request -->

##### Target release

<!-- please tell us where you would like your code to appear (e.g. v2.4): -->
I would like my code to appear in release __XXXXX__

##### Type of contribution

<!--
  Please select the type of your contribution among these:
  (Change [ ] to [X] to tick an option)
-->
- [ ] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [ ] changes to a module not authored by you
- [ ] new module contribution or edit of a module authored by you

##### Copyright

<!--
  In case you picked one of the first two choices
  MAKE SURE TO TICK ALSO THE FOLLOWING BOX
-->

- [ ] I agree to transfer the copyright of the code I have written to the PLUMED developers or to the author of the code I am modifying.

<!--
  In case you picked the third choice (new module authored by you)
  MAKE SURE TO TICK ALSO THE FOLLOWING BOX
-->

- [ ] the module I added or modified contains a `COPYRIGHT` file with the correct license information. Code should be released under an open source license. I also used the command `cd src && ./header.sh mymodulename` in order to make sure the headers of the module are correct.

##### Tests

<!--
  Make sure these boxes are checked. GitHub Actions tests are run automatically
  and their results are shown at the bottom of your pull request.
-->

- [ ] I added a new regtest or modified an existing regtest to validate my changes.
- [ ] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

<!--
  After your branch has been merged to the desired branch and then to plumed2/master, and after the
  plumed official manual has been updated, please check out the coverage scan at
  http://www.plumed.org/coverage-master
  In case your new features are not well covered, please try to add more complete regtests.
-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3d3c0d22b231`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
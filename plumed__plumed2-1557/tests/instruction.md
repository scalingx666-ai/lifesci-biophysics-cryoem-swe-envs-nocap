# Issue

**Title:** openaccPTM: fix MPI results, uninitialised derivatives, fewer transfers

## Description

Critical fixes:
- With MPI, rank 0 computed all tasks, broadcast the values and then (unless SERIAL) summed them over the ranks, multiplying the values by the number of ranks; with SERIAL the ranks other than 0 got zero forces. Rank 0 now computes and the others only receive (Bcast).
- The force kernel read device derivatives that the action never wrote (uninitialised acc_malloc memory, while the CPU task manager uses a zeroed buffer). SphericalHarmonic::calc now sets the value and derivatives of the imaginary m=0 component, and the task manager clears the derivative buffer when it is reallocated or resized.
- KDE KERNEL=DISCRETE USEGPU built "KDE_DISCRETE ACC" (space) and failed.

Performance:
- Pbc, argument bookkeeping and task list are uploaded only when they change; the input data of runAllTasks is reused in applyForces.
- Value stash and forces are cleared on the device instead of host fill plus copy.
- memoryManager grows by at least 50% and checks acc_malloc.
- Value kernel runs one task per thread (gang vector); empty kernels are skipped.

Also: rt-GPU-PLANE-bias uses SUM before BIASVALUE (vectors are rejected since the bias review) with references from the CPU run; README fixes.

Claude-Session: https://claude.ai/code/session_016vwxtV1Z8uaWMmp9UEMytj

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
  Make sure these boxes are checked. For Travis-CI tests, you can wait for them
  to be completed monitoring this page after your pull request has been submitted:
  http://travis-ci.org/plumed/plumed2/pull_requests
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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `30b75d4b7a11`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
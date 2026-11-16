# Issue

**Title:** ActionWithVector: copy values and forces to/from the stash in memory …

## Description

…order

stashToValuesImpl and forcesToStashImpl looped over the components and, for each component, over the tasks, so the stash (which holds the components of a task contiguously) was accessed with a stride of ncomponents. For FunctionOfMatrix with many components, e.g. the 26 spherical harmonics of Q6 on matrices of about 60000 elements, each element was a different cache line (together 23% of the samples of Q6). The loops are now over the tasks and, for each task, over the components; a single component is a plain loop, and the OpenMP region is opened only with more than one thread.

plumed benchmark (two kernels in the same run, 2000 atoms): Q6 on 1000 atoms 0.83 (one or two threads); COORDINATIONNUMBER (ActionWithMatrix, which already copies by rows) and a vector of 1000 DISTANCE: unchanged within the noise. All the regtests pass.

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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6e36e7978074`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
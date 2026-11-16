# Issue

**Title:** fix stale ABMD force when reaching new minimum

## Description

<!--
  Feel free to delete not relevant sections below
-->

##### Description

This pull request fixes an issue in ABMD::calculate(), where the output force is not reset when the CV reaches a new minimum distance from the target.

The Bias class stores the output forces between calculation steps. When ABMD updates the running minimum, it does not call setOutputForce(), so the previously calculated force can still be applied even though the bias energy and reported force2 are zero.

The fix consists of adding setOutputForce(i,0.0) when a new minimum is reached.

A regression test has been added in regtest/basic/rt-abmd-stale-force/

The test uses a two-atom distance collective variable with TO=0, KAPPA=1, and NOISE=0, following the sequence of distances 2 → 3 → 1.

With the original implementation, the third frame incorrectly retains atomic forces of +30 and -30 despite having zero bias energy. With the correction, both atomic forces are zero, as expected

##### Target release

I would like my code to appear in release v2.9 (the next applicable maintenance release).

##### Type of contribution

<!--
  Please select the type of your contribution among these:
  (Change [ ] to [X] to tick an option)
-->
- [X] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [ ] changes to a module not authored by you
- [ ] new module contribution or edit of a module authored by you

##### Copyright

<!--
  In case you picked one of the first two choices
  MAKE SURE TO TICK ALSO THE FOLLOWING BOX
-->

- [X] I agree to transfer the copyright of the code I have written to the PLUMED developers or to the author of the code I am modifying.

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

- [X] I added a new regtest or modified an existing regtest to validate my changes.
- [ ] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

##### Validation
Tested using PLUMED 2.9.6 on branch v2.9.
The new regression test fails with the original implementation because of the stale atomic forces.
The same regression test passes after applying the correction.
Bias energy remains unchanged.
Verified the source changes with "git diff --check"

<!--
  After your branch has been merged to the desired branch and then to plumed2/master, and after the
  plumed official manual has been updated, please check out the coverage scan at
  http://www.plumed.org/coverage-master
  In case your new features are not well covered, please try to add more complete regtests.
-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6dde2acc96a1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
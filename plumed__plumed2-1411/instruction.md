# Issue

**Title:** Fixed bug in histogram that causes LOGWEIGHTS to be ignored if NORMALISATION not present

## Description

##### Description

HISTOGRAM now crashes if you use LOGWEIGHTS without NORMALISATION.  I have also added material to the documentation for this action that recommends users to use the new syntax with KDE and ACCUMULATE instead of HISTOGRAM.  This new syntax gives them more control over what PLUMED is calculating.

I have also added documentation here to tell users to use the new syntax instead of this old action.  

Note the c++ code I have added here is the same as PR #1410 but on master and with additional documentation added in the manual

##### Target release

<!-- please tell us where you would like your code to appear (e.g. v2.4): -->
I would like my code to appear in release 2.11

##### Type of contribution

<!--
  Please select the type of your contribution among these:
  (Change [ ] to [X] to tick an option)
-->
- [x] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [ ] changes to a module not authored by you
- [ ] new module contribution or edit of a module authored by you

##### Copyright

<!--
  In case you picked one of the first two choices
  MAKE SURE TO TICK ALSO THE FOLLOWING BOX
-->

- [x] I agree to transfer the copyright of the code I have written to the PLUMED developers or to the author of the code I am modifying.

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

- [x] I added a new regtest or modified an existing regtest to validate my changes.
- [x] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

<!--
  After your branch has been merged to the desired branch and then to plumed2/master, and after the
  plumed official manual has been updated, please check out the coverage scan at
  http://www.plumed.org/coverage-master
  In case your new features are not well covered, please try to add more complete regtests.
-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d404d26a8f30`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Fix 1417 again

## Description

##### Description

@GiovanniBussi I think this should fix isssue #1417.  I took your original fix kept the first two commits so kept your test.  I removed the third commit that had the changes to the code, as they were not fixing the problem.  I thus closed your PR and replaced it with this one.

The way the code was working (and why it was not doing what was expected) was as follows:

1. You get a pointer containing the timestep from PLUMED and set a PLMD::Value equal to the timestep.
2. When you set the PLMD::Value  you do all the schenanigans with the float to double conversion.
3. You then update the units, which resets the value of the PLMD::Value to the value from the MD code (multiplied by the conversion factor), so all the schenanigans that were done are lost.

I have changed it so in point 3, you multiply the quantity stored in the PLMD::Value by the conversion factor, so it should work now.  Note that this resetting of the units is performed only if you pass a quantity that is fixed throughout the simulation and set before the MD loop starts.  

##### Target release

<!-- please tell us where you would like your code to appear (e.g. v2.4): -->
I would like my code to appear in release 2.10

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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f9937549d79b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Fix bug with adjmat/rt-matmult on intel

## Description

##### Description

Hi @carlocamilloni 

You asked me to look into this bug with adjmat/rt-matmult ages ago and I haven't really done anything till now (sorry).  My rather feeble defence was that I didn't really know what to do ;-)

Anway, I had a brain wave this morning.  The calculation is doing the following:

1. Compute a contact matrix
2. Square the contact matrix (i.e. with a propert matrix product not an element-wise product)
3. Compute the first eigenvalue
4. Print the eigenvalue

I thought if I printed out on Github the contact matrix and its square I would at least have a clue as to where things are going wrong on the intel machine.  In other words, I would see whether the matrix or its square was wrong.  Problem is that when I did just that it started passing the test on the Intel machine.    

I checked the operations PLUMED is doing against NumPy and it seems to be doing the matrix algebra correctly.

I seem to remember that you said that you can also get this to fail on your machines.  Was wondering if you wanted to try running this version again.  We could just merge this PR and perhaps that will solve the issue with intel.  That feels like an unsatisfactory solution, though.

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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0933c76076bf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Fixed CONVERT_TO_FES to make it more back compatible with earlier versions of PLUMED.

## Description

##### Description

CONVERT_TO_FES is now a shortcut action in PLUMED 2.10 that uses CUSTOM to convert to take the negative logarithm of a histogram and to multiply that by k_B T.  However, I made a mistake when adapting the MINTOZERO command to work with the new version of PLUMED.  To be clear, the shortcut that does this calculation works like this:

````
# Use -k T log P(s) to get the free energy from the histogram (h)
fu: CUSTOM ARG=h FUNC=-2.5*log(x) PERIODIC=NO
# Find the location of the minimum in the free energy surface that we have just calculated
m: FIND_GRID_MINIMUM ARG=fu 
# Subtract the minimum in the free energy from the grid so that the minimum has a free energy of zero.
f: CUSTOM ARG=fu,m.optval FUNC=x-y PERIODIC=NO
````

Because I am too clever for my own good, FIND_GRID_MINIMUM uses spine interpolation and a conjugate gradient algorithm to find the coordinates of the actual minimum of the free energy surface.  However, the earlier implementation of MINTOZERO just searched the value of the free energy on the various grid points, found the lowest and used that as the origin.

With this change, I have thus adjusted the shortcut so that if you use the MINTOZERO option, you just find the lowest free energy point on the grid and use that for the position of the origin.  The behaviour with this option thus recovers what was done in old versions of the code.

If you want to do the actual minimisation, you can use the FINDMINTOZERO, which does the conjugate gradient minimisation.

In rt-fes-periodic there were two CONVERT_TO_FES commands that use the MINTOZERO option.  I changed one of these commands to FINDMINTOZERO (this is the one where the reference is unchanged).  For the other, I kept it as MINTOZERO and checked that the values that you get out with this option are the same as what they would be in older versions of PLUMED.  I thus ensured that we are properly maintaining back compatibility.

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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3648da5c3118`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
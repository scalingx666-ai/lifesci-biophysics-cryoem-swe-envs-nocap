# Issue

**Title:** Add flag to do analysis and printing at end of run even if not on STRIDE

## Description

##### Description

This is an improvement to the way that actions like DUMPGRID, ACCUMULATE and COLLECT work.  Suppose that you have a trajectory with 35 frames.  Prior to this commit there were two ways you could analyse this data.  In the first you can analyse all 35 frames  in your trajectory something like this:

````
d: DISTANCE ATOMS=1,2
h: HISTOGRAM ARG=d ....
DUMPGRID ARG=h FILE=colvar
````

The second way of doing this involves outputting the HISTOGRAM evevery 10 steps like this:

````
d: DISTANCE ATOMS=1,2
h: HISTOGRAM ARG=d ....
DUMPGRID ARG=h FILE=colvar STRIDE=10
````

Importantly, however, this second method ignores the last 5 frames in the trajectory because 35 is not divisible by 10.  You can now add the DO_AFTER_FINAL_STEP flag like this:

````
d: DISTANCE ATOMS=1,2
h: HISTOGRAM ARG=d ....
DUMPGRID ARG=h FILE=colvar STRIDE=10 DO_AFTER_FINAL_STEP
````

This will output 4 files containing the histogram, these are output on steps 10, 20, 30 and at the end of the trajectory.  Similar functionality has been added in COLLECT.  This new functionality in COLLECT means you can COLLECT data between strides where the data is cleared.  Analysis is performed at the end the run even if you have not collected `clear/stride` pieces of data as you would have done in the previous steps

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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `08a6de70c082`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
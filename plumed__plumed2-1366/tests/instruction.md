# Issue

**Title:** _guessplumedroot now deletes the temporary file used to store the plumed output

## Description

<!--
  Feel free to delete not relevant sections below
-->

##### Description

In the [mkstemp documentation](https://docs.python.org/3.9/library/tempfile.html#tempfile.mkstemp) there is the following sentence:
> Unlike TemporaryFile(), the user of mkstemp() is responsible for
> deleting the temporary file when done with it.

So I changed the routine to use the higher level NamedTemporaryFile, to it will automatically delete the tmp file every time I run some test (or the user tries to create a new input with the InputBuilder)



<!-- describe here your pull request -->

##### Target release

<!-- please tell us where you would like your code to appear (e.g. v2.4): -->
I would like my code to appear in release __v2.9__

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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `409500917914`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
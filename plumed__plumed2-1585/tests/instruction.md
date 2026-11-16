# Issue

**Title:** Fixed bug in KDE and added option to ignore components in READ

## Description

##### Description

This commit fixes a bug in KDE.  The problem occured if you were setting the bandwidth equal to a value like this:

````
val: READ VALUES=a FILE=colvar
val_b READ VALUES VALUES=b FILE=colvar

KDE ARG=val BANDWIDTH=val_b ...
````

This bug appeared in a form of input for this action that was not tested previously so I have added a test to make sure it doesn't break again in future.

I also added an additional feature to READ that might be useful.  Currently, you have to use the READ command like this:

````
v: READ ARG=p.x FILE=colvar
PRINT ARG=v.x STRIDE=1
````

I have added the option to use it like this:

````
v: READ IGNORE_COMPONENT ARG=p.x FILE=colvar
PRINT ARG=v STRIDE=1
````

The component of the read in action is ignored in the output value so you can just refer to it using the label of the action.

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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9855ecebecdc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
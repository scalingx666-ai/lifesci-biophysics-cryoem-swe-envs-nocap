# Issue

**Title:** Adding gh integration to the codecheck workflow

## Description

<!--
  Feel free to delete not relevant sections below
-->

##### Description

These modifications add a feature on top of the CI of codecheck: it produces a "standard" (accordingly to GH) SARIF file that tells github the location of the errors found by cppcheck and plumed check and curated by the `codecheck` script.

The old `make codecheck` behaviour has not changed, if run in local mode it does not engage the new python script, and if python is not present it skips the conversion.

>[!NOTE]
>The new python script is a pure "standard library" python script, no dependency on any external module. It post processes the `${FATAL_FILE}` produced by codechek. It also "greps" the descriptions of the errors from plumedcheck to give the our explanation of the error.

I modified some of the descriptions of the errors in plumedcheck to give them a more "aesthetic look" in github without breaking how they appear in the developer manual: the first line appears as a "title" in the error report and then it gets added as the first phrase of the "bulk" description.

On top of this I added also the sarif file that ruff produces automatically, if we discover it to be too much noisy it can be axed easily.

###### An example::

If you look at this PR:  I vandalized some code, so you can see how this brings some errors in the PR discussion (with their [fancy page](https://github.com/Iximiel/plumed2/security/code-scanning/1248) ). They are posted by the "github-advanced-security" bot, that make them scarier. I have to check if it is possible to change that.
Some errors, like the astyle ones, that concern the whole file are not reported in the discussion, but gh has a report page https://github.com/Iximiel/plumed2/security/code-scanning?query=is%3Aopen+pr%3A49 that contains everything that has been reported.

##### Target release

<!-- please tell us where you would like your code to appear (e.g. v2.4): -->
I would like my code to appear in release __v2.11__ I do not think backporting this might be useful

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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2e6f01fae6e5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
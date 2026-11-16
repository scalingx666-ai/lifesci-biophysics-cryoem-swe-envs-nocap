# Issue

**Title:** Plugins documentation

## Description

##### Description

This is my suggestion for how to include documentation for the plugins that Daniele wrote in the PLUMED manual.  

My idea was to add a table at the bottom of the modules page that has the plugins in.  This will look like this:

<img width="1819" height="1018" alt="plugins-page" src="https://github.com/user-attachments/assets/260e4598-243a-44db-8e7a-9598618df931" />

(I can adjust the authors in the table to whatever people feel they should be.)

If you then go to the pycv page you see the following:

<img width="1819" height="1018" alt="pycv" src="https://github.com/user-attachments/assets/aa86e62c-fb44-4eb0-86ec-533d64f3a24b" />

which is a rendered version of whatever was in the `README.md` that is in this particular plugins directory with the titles that are shared by the other pages in the manual.  I can't find things in the nest that use the plugin.  For the tutorials page, I am allowing plugin developers to use a custom search term to search through the lessons to find the appropriate tutorials.

I was looking through the README files and I think that we may want to refine them a bit with Daneiele.  I think that the one for the openaccPTM is not correct anymore.  It may be worth doing this as part of this PR.

It is also worth noting that as I did this I noticed the following announcement:

https://squidfunk.github.io/mkdocs-material/blog/2026/02/18/mkdocs-2.0/

To be clear, we are using materials for mkdocs to build the documentation so this may well affect us.  Any thoughts on what we should do here.

##### Target release

<!-- please tell us where you would like your code to appear (e.g. v2.4): -->
I would like my code to appear in release 2.11

##### Type of contribution

<!--
  Please select the type of your contribution among these:
  (Change [ ] to [X] to tick an option)
-->
- [x] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [x] changes to a module not authored by you
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

- [ ] I added a new regtest or modified an existing regtest to validate my changes.
- [x] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

<!--
  After your branch has been merged to the desired branch and then to plumed2/master, and after the
  plumed official manual has been updated, please check out the coverage scan at
  http://www.plumed.org/coverage-master
  In case your new features are not well covered, please try to add more complete regtests.
-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `adcab8d46d37`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
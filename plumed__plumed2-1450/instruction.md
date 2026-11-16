# Issue

**Title:** Changes to avoid sparse matrix lookup operations unless they are absolutely necessary that make FunctionOfMatrix faster

## Description

##### Description

PLMD::Value handles two types of matrix:

* Dense matrices where you have access to every element.
* Sparse matrices where each row has values in a subset of the columns.

The sparse matrices occur naturally in the context of calculating a contact_matrix.  All the central atoms that are specified have some neighbours so a small subset of the columns in each row are non-zero.  However, there are likely  no central atoms that have no neighbours so no row is entirely filled with zeros.

If you have a sparse matrix you have to do some expensive look-up operations to get element i,j.  Obviously, you wan to avoid these operations if you have a dense matrix!  This change ensures that we do indeed avoid performing these operations when the matrix is dense.  We weren't avoiding those expensive lookup operations in older versions of the code, which was adding (unecessary) computational expense.

To make this work I had to make a small change in LinkCells.  In essence, before this change if you computed a contact matrix that was not sparse (because the cell was large relative to D_MAX) the order the atoms are in is  shuffled because of the link cell procedure.  The net result is that you have a dense matrix where you have to do expensive look-up operations.  The change to LinkCells essentially ensures that the atom positions are kept in the right order if the number of atoms in the neighboring cells is not less than the total number of atoms.  This change ensures that you also don't need to do lookup operations if your CONTACT_MATRIX is dense.  

In other words, this change ensures that we are only doing the sparse matrix lookup operations when they are actually needed.

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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a740f1925cbe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
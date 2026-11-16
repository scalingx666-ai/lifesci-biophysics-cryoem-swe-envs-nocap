# Issue

**Title:** Martini3 SAXS formfactors

## Description

## Description

This pull request adds support for using Martini 3 protein form factors in the `SAXS` action, allowing users to select between Martini 2.2 (the previous default) and Martini 3 form-factor sets via a new `MARTINI_FF` keyword.

* Added the `MARTINI_FF` keyword to the `SAXS` action, allowing users to choose between Martini 2.2 (`MARTINI_FF=2`, default) and Martini 3 (`MARTINI_FF=3`) protein form factors. The code validates the keyword and applies the correct parameter set. (`src/isdb/SAXS.cpp`) [[1]](diffhunk://#diff-88555506f8eb34c4a9701bd282082f748e83abc2a049fc4e4979642b8985d515R460) [[2]](diffhunk://#diff-88555506f8eb34c4a9701bd282082f748e83abc2a049fc4e4979642b8985d515R547-R555) [[3]](diffhunk://#diff-88555506f8eb34c4a9701bd282082f748e83abc2a049fc4e4979642b8985d515R917-R921)
* Implemented a new method `getMartiniFFparam_v3` to provide the Martini 3 protein bead form-factor coefficients and updated the bead enumeration to include new beads present only in Martini 3. (`src/isdb/SAXS.cpp`) [[1]](diffhunk://#diff-88555506f8eb34c4a9701bd282082f748e83abc2a049fc4e4979642b8985d515L259-R263) [[2]](diffhunk://#diff-88555506f8eb34c4a9701bd282082f748e83abc2a049fc4e4979642b8985d515R3285-R3559)
* Updated the user documentation and changelog to describe the new `MARTINI_FF` option and its behavior. (`src/isdb/SAXS.cpp`, `CHANGES/v2.11.md`) [[1]](diffhunk://#diff-88555506f8eb34c4a9701bd282082f748e83abc2a049fc4e4979642b8985d515L81-R82) [[2]](diffhunk://#diff-df4646a719abcc48ae1910f28def584df17694a83330225e28e68882dd91efe2R9)
* Added a new regression test for the `SAXS` action with Martini 3 form factors, including configuration, reference output, and input files. (`regtest/isdb/rt-saxs-martini3/`) 

These changes ensure users can now select the latest Martini 3 protein form factors for SAXS calculations in PLUMED, with appropriate documentation and testing.

## Target release

I would like my code to appear in release v2.11

## Type of contribution

<!--
  Please select the type of your contribution among these:
  (Change [ ] to [X] to tick an option)
-->
- [ ] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [x] changes to a module not authored by you
- [ ] new module contribution or edit of a module authored by you

## Copyright

- [x] I agree to transfer the copyright of the code I have written to the PLUMED developers or to the author of the code I am modifying.

- [ ] the module I added or modified contains a `COPYRIGHT` file with the correct license information. Code should be released under an open source license. I also used the command `cd src && ./header.sh mymodulename` in order to make sure the headers of the module are correct. 

## Tests

- [x] I added a new regtest or modified an existing regtest to validate my changes.
- [ ] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

<!--
  After your branch has been merged to the desired branch and then to plumed2/master, and after the
  plumed official manual has been updated, please check out the coverage scan at
  http://www.plumed.org/coverage-master
  In case your new features are not well covered, please try to add more complete regtests.
-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4a2274459575`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
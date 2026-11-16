# Issue

**Title:** Add more atom type SMARTS defs, new bond, angle and dihedral parameters to `oplsaa.xml`

## Description

### PR Summary:

The purpose of the PR is to expand available atom types with definitions, bond, angle and dihedral parameters from the ITP files and parameters provided [here](https://virtualchemistry.org/ff.php) and as discussed in [this paper](https://pubs.acs.org/doi/10.1021/ct200731v)

No parameters (epsilons, sigmas, spring constants, etc) were changed for existing entries in `oplsaa.xml`. Some slight modifications to existing atom type SMARTS definitions were made to handle conflicts arising by adding new definitions. All newly added bond, angle and dihedral parameters are at the bottom of their respective sections of the XML, with a comment block indicating the source.

~45 new molecules were added to the opls validation tests

Note: A few `.top` files in the `opls_validation` set of molecules were changed to both reflect the files provided in the link above, and to be more accurate (from what I can tell).

There are still a couple of things remaining, and a few more atom type definitions to sort through, but I wanted to get the PR out sooner rather than later.

### PR Checklist
------------
 - [x] Includes appropriate unit test(s)
 - [x] Appropriate docstring(s) are added/updated
 - [x] Code is (approximately) PEP8 compliant
 - [x] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6ffaafbf14a1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
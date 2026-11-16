# Issue

**Title:** Fix RelativeFEPSetup bug preventing user from controlling the padding in solvent phase

## Description

## Description

<!-- Describe your changes in detail. -->
This PR fixes a bug that prevents the user from choosing the padding size (for adding solvent) when the phase is solvent. 

Note: I found this when I was trying to write the small molecule repex internal consistency tests.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->
Currently the code always uses a 9 angstrom padding whenever the phase is solvent. I removed the code that does this.

<!-- Replace ??? with the issue number that this pull request resolves. -->

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->
Added `tests/test_relative_setup.py::test_relative_setup_solvent_padding`

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Fix bug in RelativeFEPSetup that prevents user from controlling the padding when solvating for solvent phase calculations. 
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8cb083e05bfd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
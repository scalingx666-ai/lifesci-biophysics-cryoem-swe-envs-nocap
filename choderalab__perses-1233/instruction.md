# Issue

**Title:** Fix spectator support

## Description

## Description

These changes remove an unnecessary call to `RelativeFEPSetup._setup_complex_phase` method, fixing the support for spectator molecules.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 
 

## How has this been tested?

Re-enabling previously skipped test for setting up bace system with spectator molecule.

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Regression error when setting up systems with spectator molecules has been fixed by avoiding early and unnecessary calls to setup methods.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ff55126f7e31`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
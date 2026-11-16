# Issue

**Title:** use beta of openeye toolkit

## Description

## Description

This PR installs openeye toolkit with the `beta` label https://anaconda.org/OpenEye/OpenEye-toolkits/files
Instead of expanding the matrix, I opted to do this as part of the openmm nightly check, since it isn't required to pass + didn't want to eat up too much CI time. 

## Motivation and context

We want to be able to check if any changes to the toolkit cause our tests to fail. We also want to be able to report bugs back to openeye.
<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7d76ddaba9cf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
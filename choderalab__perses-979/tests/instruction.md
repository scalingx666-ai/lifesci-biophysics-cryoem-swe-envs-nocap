# Issue

**Title:** Fix real time mbar disabling appending uncorrelated endstates

## Description

## Description

In order to have the real time analysis YAML output with estimates for MBAR calculations and performance of the simulation, we had to disable uncorrelated end states creation.

Minor clean up: No minimization needed for `HybridCompatibilityMixin`.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

tested locally. Waiting for benchmarks runs for real-case performance testing.

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Output for real time mbar calculations and simulation performance in YAML format.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `54b0a3d0bc94`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
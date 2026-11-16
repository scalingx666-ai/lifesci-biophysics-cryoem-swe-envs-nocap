# Issue

**Title:** Change rest factory defaults and clean up

## Description

## Description

<!-- Describe your changes in detail. -->
- Turn the long range correction off by default for CustomNonbondedForce_sterics and turn it on for the tests
- Change default w_scale and rest_radius to 0.3
- Remove unnecessary f string in global parameter strings
- Edit docstrings

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->
Updates defaults, cleans up some unnecessary code, and updates docstrings to reflect changes made previously.

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->
Existing tests in `tests/test_relative.py::test_RESTCapableHybridTopologyFactory_energies_{GPU}()`

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c97edd4ebf00`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
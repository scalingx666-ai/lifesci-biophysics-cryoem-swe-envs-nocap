# Issue

**Title:** Introduce new function for creating unsampled endstates

## Description

## Description

<!-- Describe your changes in detail. -->
- Introduces a new `create_endstates()` function called `create_endstates_from_real_systems()` that generates unsampled endstates for the `HybridTopologyFactory` and `RESTCapableHybridTopologyFactory` using each's real systems (old and new systems). 
    - Note that `create_endstates()` only worked for `HybridTopologyFactory`
- Adds necessary tests for the above feature
- Updates `validate_endstate_energies_md()` to accept a `step_freq` argument
    - Note that this is necessary because previously, if we specified `n_steps=10`, the function would save snapshots every 1 ps, but we want it to save every snapshot.


~Note that one of the lines in `tests/test_relative.py::run_unsampled_endstate_energies()' can be removed once we merge this PR: 
^ I've removed the line now that #1022 has been merged

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->
Tested in `tests/test_relative.py::test_unsampled_endstate_energies{_GPU}`

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e3f65eafaef2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Update HybridCompatibilityMixin to handle RESTCapableHybridTopologyFactory and add repex self-consistency tests

## Description

## Description

<!-- Describe your changes in detail. -->
- Updated `HybridCompatilityMixin` to automatically detect which type of factory (`HybridTopologyFactory` or `RESTCapableHybridTopologyFactory`) was inputted and create a sampler depending on the factory type. 
- Added self-consistency tests for dipeptide systems (running repex with the new `RESTCapableHybridTopologyFactory`):
   - Neutral mutation: ALA->THR vs THR->ALA
   - Charge changing: ARG->ALA->ARG vs LYS->ALA->LYS (see docstring of test for explanation on why these particular transformations are necessary)

TODO:
- I put the tests in `test_samplers.py` -- not sure if this is the right place -- may want to move it somewhere else and/or make it an example?
- Not sure if the tests in `test_samplers.py` are even working -- should we delete everything else in here?
- `n_iterations` should be bumped up to 1000 and it should be run on a gpu (@mikemhenry : Can you help with this?)

## Motivation and context

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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ef352011e088`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
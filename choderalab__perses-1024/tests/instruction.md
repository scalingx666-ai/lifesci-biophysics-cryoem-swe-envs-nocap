# Issue

**Title:** Enhance point mutation executor

## Description

## Description

<!-- Describe your changes in detail. -->
- Update docstrings and comments
- Fix broken example in docstring 
- Remove `box_dimensions` as input to `PointMutationExecutor`
- Accept `padding` as input to `PointMutationExecutor`
- Accept `box_shape` as input to `PointMutationExecutor`
- Remove `phase` as input to `PointMutationExecutor` and replace with `is_vacuum`
- Remove `solvate` as input to `PointMutationExecutor` and replace with `is_solvated`

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->
The above changes 1) improve the readability/usability of the `PointMutationExecutor` and 2) Updates the solvation code to be more practical (in my experience, `box_dimensions` was never used, but I did want to change the padding and box_shape) 

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->
Existing tests: `tests/test_resume.py` and `tests/test_relative_point_mutation_setup.py`

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8d728bbf541e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
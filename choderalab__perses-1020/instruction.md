# Issue

**Title:** Speed up functions for retrieving old/new positions from hybrid positions

## Description

## Description

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->

The existing test `test_relative.py::test_position_output()` passes.

I added a timer to this test and here are the times with the old approach vs new approach (this PR):
- with old approach:
	- old_positions(): 0.04454398155212402
	- new_positions(): 0.04473400115966797
- with new approach
	- old_positions(): 0.001668691635131836
	- new_positions(): 0.001196146011352539

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9c9902e537a9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
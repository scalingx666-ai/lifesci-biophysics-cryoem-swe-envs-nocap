# Issue

**Title:** Fixes to visualization module

## Description

## Description
<!-- Describe your changes in detail. -->
Make fixes to the visualization module related to:
- Pymol frames are 1-indexed, but the module had been assuming it was 0-indexed
- Bug: `self._unique_old/new` should be `self._unique_old/new_selection`
- Renaming an object after loading isn't working, so instead, just load the objects as `old` and `new` directly
- Allow color of old residue to be different from the color of the new residue
- Change background protein transparency
- Remove "hide spheres" when we are using stick representation (shouldn't be necessary)
- Do not allow stick/sphere transparencies to fade to transparency 1.0 (completely transparent) -- make the max transparency 0.7, as there are weird visual artifacts when it is 1.0 (the stick/sphere looks white)
- Clarifications to comments/docstrings

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
Resolves #???

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9beb9b060289`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
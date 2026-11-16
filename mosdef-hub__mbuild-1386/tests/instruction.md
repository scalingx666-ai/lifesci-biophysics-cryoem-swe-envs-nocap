# Issue

**Title:** Add excluded inner radius parameter to `Cylinder` and `Sphere` constraints.

## Description

### PR Summary:

This extends the `CylinderConstraint` and `SphereConstraint` to have an optional excluded inner core, resulting in channel/pipe an hollow sphere type of accessible volumes. This also fixes a small bug in `SphereConstraint`, which wasn't using the origin to calculate distance from the center


### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `23f168ccef29`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
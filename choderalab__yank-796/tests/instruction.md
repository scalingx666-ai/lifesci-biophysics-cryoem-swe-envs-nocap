# Issue

**Title:** Add in Region selected atoms to the Restraint class

## Description

This PR adds a number of changes to the restraint functionality as continuation of #793 and #794

This PR adds the ability for Restraint classes to accept Topography Region-based logic as part of the selection string, as well MDTraj strings. This also extends the Topography to handle complex region selection through the `and` and `or` operators.

Main logical question: Should we *remove* the ability for DSL strings to be passed as direct atom selections in Restraints and force it to use Topography Regions? That would clean up the code, but then offload the DSL processing exclusively to Topology and remove the functionality from restraints as a standalone.

There is a break to the `RadiallySymetricRestraint` class API which went unused for the most part, but still a break so version updated to 0.19.0.

~This is a WIP for the following reasons, although the functionality changes should all be in now, allowing discussion and review~ No longer a WIP as all items have been checked off.
* [x] OpenMMTools 0.13.2 is required (not released)  due to extended `math_eval`, tests will not run until thats released
 * [x] I'm considering extending the Schema validation for restraints to ensure Regions are previously defined, but that will be hard considering that the current Schema accepts any input so arbitrary parameters can be passed. Not sure how to check
 * [x] The tests should be extended to test the Region selection code
 * [x] The logic should be reviewed to ensure I am not missing corner cases
 * [x] The Restraints doc-strings need updated
 * [x] The website docs need the "whats new" updated

 Next on the list is how do we parse SMILES strings

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `50a5b6386368`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
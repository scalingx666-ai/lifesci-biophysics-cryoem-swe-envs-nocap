# Issue

**Title:** Fix cutoff and lifting parameters in `RESTCapableHybridTopologyFactory`

## Description

## Description

<!-- Describe your changes in detail. -->
This PR addresses the following problem: 

The `RESTCapableHybridTopologyFactory` is not performing lifting to the 4th dimension correctly in vacuum. We are maximally lifting to 100 nm, which is much too large. The maximal lifting distance is currently defined by `w_scale * r_cutoff`. For vacuum, we set `r_cutoff` to be 100 nm, which is the distance we want to use when calling `CustomNonbondedForce.setCutoffDistance()`, but we do not want the maximal lifting distance to be that large, therefore, we need to remove the dependence of the maximal lifting distance on `r_cutoff`. 

In fact, we actually don't need the maximal lifting distance to depend on 2 parameters, we can use just 1 parameter, which I'll call `w_lifting`, that defines the maximal lifting distance in nanometers.

This PR does the following: 
- Renames `r_cutoff` to `cutoff`. `cutoff` is copied from the real system and if the nonbonded method is vacuum, `cutoff` is set to 100 nm. `cutoff` is only used when setting the cutoff distance in the `CustomNonbondedForce`s via `setCutoffDistance` and not in the custom expression when defining lifting expressions.
- When defining the lifting expressions, use a single user-controlled parameter (`w_lifting`) 

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Bug fix in the `RESTCapableHybridTopologyFactory` lifting expression -- fixed by separating the cutoff distance from the lifting distance.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `28885ab11d2e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
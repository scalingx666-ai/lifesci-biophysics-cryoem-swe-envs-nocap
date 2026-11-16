# Issue

**Title:** Minor improvements to point mutation executor

## Description

## Description

<!-- Describe your changes in detail. -->

Here, I've included some minor fixes to the `PointMutationExecutor`:
1. Make sure reverse geometry proposal happens right after forward geometry proposal.
2. Add naked charge fix
3. Fix `complex_positions` formatting issue 
See details below.


## Motivation and context

<!--- Why is this change required? What problem does it solve? -->


Changes:
1. Move the line for generating the reverse geometry proposal s.t. it is directly after the forward proposal line. We want to do this to make sure that the forward and reverse proposals are executed before the counterion code, otherwise we end up running the forward proposal using the unmodified new system and the reverse proposal using the modified (by counterion code) new system. 
2. Add a fix that adds padding if there are naked charges. John suggested this as amber forcefields allow for naked charges, e.g. on hydroxyl hydrogens
3. Resolve a formatting problem for `complex_positions` -- When visualizing the complex positions after adding solvent, the protein was translated away from the solvent. This is because `complex_positions` is a numpy array, but openmm stores positions as Vec3 objects. When calling `modeller.addSolvent()`, there is likely a translation step that moves the solvent to the protein, but this step can only be done when the positions are in the Vec3 format. The solution here is to convert `complex_positions` to Vec3 format before adding solvent. 

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->
As long as [this test](https://github.com/choderalab/perses/blob/master/perses/tests/test_relative_point_mutation_setup.py) passes, we should be fine. 

To make sure change 2 is working correctly, I manually inspected the PDB in PyMOL to make sure the protein positions are no longer translated away from solvent.

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `02f325db8f0a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
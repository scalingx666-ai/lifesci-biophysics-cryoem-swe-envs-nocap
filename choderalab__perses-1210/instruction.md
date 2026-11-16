# Issue

**Title:** Store initial and final topologies for all phases -- small molecule pipeline

## Description

## Description

These changes make serialization of initial (old) and final (new) topologies for complex and solvent phases for the small molecule pipeline, adding the `RelativeFEPSetup._serialize_topologies` method which is called at the end of the constructor.

Note that while these changes do NOT break the API, it is a change in the behavior since now the stored objects are different.

## Motivation and context

For extracting trajectories for debugging or visualization purposes, it is important to have the topologies available in PDB for the different phases and the initial and final states of the transformation.

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

Test written, need to check locally that the files make sense.
<!--- Please describe in detail how you tested your changes. -->

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Storage of topologies for initial and final states of the transformation, for all phases of the simulation. Useful for extracting trajectories.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4834ff792097`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
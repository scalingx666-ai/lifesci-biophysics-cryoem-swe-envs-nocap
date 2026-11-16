# Issue

**Title:** Store new and old positions and maps

## Description

## Description
This allow the serialization of new and old positions in pdb files as well as atom mappings in numpy object file, when creating hybrid topologies. 
<!-- Describe your changes in detail. -->

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 
## How has this been tested?
Tested using [minimal-neq-sprint.tar.gz](https://github.com/choderalab/perses/files/6948343/minimal-neq-sprint.tar.gz) by running `python 05-prepare-single-transformation.py 0`

<!--- Please describe in detail how you tested your changes. -->

## Change log
Addresses (issue #752 ) by storing positions and maps for new and old molecules when creating a hybrid topology.
<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cfbdc7a020fc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
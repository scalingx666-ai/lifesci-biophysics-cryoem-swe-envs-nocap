# Issue

**Title:** Specifying hybrid topology factory (htf) from input file

## Description

## Description

Created a common API point to handle the Hybrid Topology Factory creation. It can handle the 'vanilla' `HybridTopologyFactory` and the new `RESTCapableHybridTopologyFactory`. 

The new parameter for the input `yaml` file is `hybrid_topology_factory` and it expects the full name of the class as a value. It raises an error if you provide an invalid name.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->


## How has this been tested?

Tested locally by running the protein-ligand example from cli.

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6aedcd0e1fa9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
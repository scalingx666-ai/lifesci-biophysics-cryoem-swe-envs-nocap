# Issue

**Title:** Fix box vector handling in topology proposal

## Description

## Description

<!-- Describe your changes in detail. -->
This PR makes sure that when a source topology is being copied to a destination topology, the source topology's box vectors are copied to the destination topology.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->
OpenMM only writes out box vectors to DCD files if the `topology.getPeriodicBoxVectors()` is not None: 

I ran a simulation and the box vectors were not saved because `htf._topology_proposal.old_topology` does not have box vectors. This PR addresses the problem by adding a line that copies the box vectors from the source to destination topology in `append_topology()`. It removes the line in `_propose()` that does this for the new topology only.


## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `686d7b3bcd06`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
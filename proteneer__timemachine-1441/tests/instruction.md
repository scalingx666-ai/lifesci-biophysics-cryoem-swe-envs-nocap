# Issue

**Title:** Improve Caching of CI Container

## Description

Improves the caching of the docker builds by storing the cache in each layer. When images are pulled from the registry later, the caching data reduces the number of layers that have to be rebuilt.

#### Minor changes:
* Fixes a warning in the docker file about inconsistent casing
* Removes Vim from the container, as it is unused. 
* Remove the docker prune call, CI no longer needs pruning.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5f41936c59bc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
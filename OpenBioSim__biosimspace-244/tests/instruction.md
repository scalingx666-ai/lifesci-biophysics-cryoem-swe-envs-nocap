# Issue

**Title:** Fix issues #241, #242, and #243

## Description

This PR...


    This removes shared properties when loading a perturbable system from file. This is needed to avoid issues when combining molecules together to form new systems. I've also added some extra logic into the GROMACS process code to check for a periodic system when updating the space property. For vacuum simulations we use a "fake" infinite box (as recommended by GROMACS) and I don't want to copy this property back if the original system contained an infinite cartesian space, which is now the default in Sire. Note that there was a typo in the existing code for `readMolecules`, which didn't cause any issues in practice since the property isn't used internally.


    This updates the error message to use the correct file names and removes the now redundant `parmed` import.


    This exposes the `FreeEnergyMixin` protocols, allowing GROMACS to run any existing protocol and intermediate lambda values, not just the end states. (This will be also be exposed for AMBER when ready.)

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Suggested reviewers:
@chryswoods

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bec58fb6db32`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
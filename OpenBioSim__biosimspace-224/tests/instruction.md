# Issue

**Title:** Make sure disulphide search is converted to a SelectorBond.

## Description

This PR  and closes OpenBioSim/biosimspace_tutorials#20 by making sure any (succesful) search for disulphide bonds is converted to a `SelectorBond`, thus is iterable. At present, if a single result is found, then it is returned as a `Bond`, hence the logic that follows the search will fail, i.e. getting an item from will extract an `Atom`, not a `Bond`.

I've tested the search logic for other bonds in a molecule, e.g. those between specific atom names, or indices. I don't (yet) have a single disulphide bond test molecule to add.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Suggested reviewers:
@chryswoods

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7869803d73a4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Remove cross-bond terms for ring-making/breaking perturbations

## Description

This PR fixes the merge code for ring-break perturbations by cleaning up (removing) spurious bonded terms from the end-state where the bond is absent, i.e. angles, dihedrals, and impropers that cross the bond. The merge code is built on the assumption of the same connectivity at each state, so terms that are unique to one state are mirrored in the other, which isn't correct when a bond is broken.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `24a914493e1f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
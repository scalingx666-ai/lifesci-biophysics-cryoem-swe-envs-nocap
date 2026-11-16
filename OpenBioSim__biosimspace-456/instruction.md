# Issue

**Title:** Fix issue #445

## Description

This PR  by correctly handling perturbable reference systems. There are multiple subtleties:

* The reference might have molecular properties that mismatch the frame, e.g. `coordinates0` rather than `coordinates`.
* The number of atoms in the trajectory might not agree with the reference system, i.e. if the trajectory was from and AMBER simulation, in which case the trajectory contains a "squashed" representation. In this case the reference needs to first be squashed to match the trajectory, then unsquashed once the specific frames of interest have been loaded.

I have tested this locally using RBFE output for AMBER, GROMACS, and SOMD, using all trajectory backends. I'm not sure what to do about unit tests, since I don't currently have a consistent test case for all engines. (AMBER isn't working locally.) Ideally I'd have a small 10 frame vacuum system simulated with all engines. I'll see if I can generate this, then update accordingly.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1a109a456516`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
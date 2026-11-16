# Issue

**Title:** Expose torsion modification kwargs

## Description

This PR exposes the dummy torsion force constant modification kwargs used by the pert file writer in the `Process.Somd` constructor. These aren't really used, but I want to enable them for debugging purposes. Currently they are set to `False` by default and can only be used if calling the pert file writer function directly.

(This isn't really a bug, since it's not used, but I did forget to expose these when adding kwargs to the process classes to capture engine specific options.)

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6b2347335ab8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
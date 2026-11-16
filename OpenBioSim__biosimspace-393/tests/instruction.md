# Issue

**Title:** Fix issue #392

## Description

This PR  by improving the robustness of the internal formal charge calculation within the `Parameters` sub-module. Now we preferentially use the format the the original molecule was loaded from, either PDB or SDF, or try both if none are present in the file formats list. This means that we can still get a result if the PDB conversion doesn't work, etc.

Note that I haven't been provided with the problem file in this case, so haven't added an additional test. All existing tests pass.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `de6629c074fc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
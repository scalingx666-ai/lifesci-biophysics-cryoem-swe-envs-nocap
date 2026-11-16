# Issue

**Title:** Fix issue #284

## Description

This PR  by using SDF as the intermediate format for `antechamber` if the original molecule was loaded from that format, otherwise PDB is used. This mirrors the functionality used for the OpenFF protocol. A more general solution could be to allow the user to specify the intermediate format, but I'm not sure if this is needed at this time.

The fix has been tested by the author of issue #284. They are unable to share the molecule in question so I haven't added a unit test at this time.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Suggested reviewers:
@chryswoods

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6d431c5b8697`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Add functionality for parsing CMAP energy records

## Description

This PR adds support for parsing CMAP energy records to the AMBER and GROMACS process classes. A unit test is added to validate that CMAP energies survive AMBER --> GROMACS format conversion and are properly handled internally by the respective engines.

Note that CI will fail on macOS and Windows due to Sire CI glitches.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6c8d5ffe8d99`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Synchronise with Exscientia remote

## Description

This PR synchronise with the [Exscientia remote](https://github.com/Exscientia/BioSimSpace) in preparation for the 2023.5.0 release.

Bizarrely, running tests locally repeatedly fails with the following exception (in a random place):

```
Tcl_AsyncDelete: async handler deleted by the wrong thread
Fatal Python error: Aborted
```

When run against `devel`, the tests pass without issue. Diffing the branches show that the are _zero_ differences between the python source code, the only differences being in the `requirements.txt` file and the Exscientia Sandpit CI configuration, which are irrelevant for the purposes of running local tests. I'll trigger the CI here to see if the same issue occurs during the builds.

Update: I made another commit to fix a typo in a test name and all of the segmentation fault no longer happens! (The typo wasn't causing the problem.) I have no idea why tests against the merge commit were failing, but adding another commit (which touches no code) appears to fix things :man_shrugging:  Clearly some `pytest` weirdness.


* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `233d87ea0e67`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
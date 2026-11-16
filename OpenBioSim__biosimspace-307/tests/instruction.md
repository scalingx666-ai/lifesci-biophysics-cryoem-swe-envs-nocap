# Issue

**Title:** Synchronise with Exscientia remote

## Description

This synchronises with the Exscientia remote ahead of the 2024.2.0 release. Annoyingly it appears that many platform specific skipif directives had been removed from the tests, so I've needed to manually re-add them. Hopefully the CI passes, otherwise I'll need to check what else might have been removed. I'll make a note to mention this since there's no reason to do this. Specific dependencies need to be present for the tests to run. Clearly they are only running their own CI on a specific platform, so thought they could remove them as some minor optimisation.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `81b48f0d2820`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
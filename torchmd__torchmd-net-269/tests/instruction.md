# Issue

**Title:** Fix inifinite recursion error on OSX

## Description

Circumvent a possible infinite recursion error in FloatCastDatasetWrapper in which getattr calls itself.
This problem only arose in OSX when a DataLoader uses more than one thread. In that case the dataset is pickled and causes an error.

This PR fixes it by making sure the getattr method in the aforementioned class does not call itself.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `96190083ab16`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
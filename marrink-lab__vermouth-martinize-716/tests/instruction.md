# Issue

**Title:** Add bb_atomname flexibility to elastic network selectors too

## Description

in #671 we added flexibility for backbone selection so we could change where the virtual go bead is placed. The equivalent for the elastic network was missed, which we implement here. The extra general.ff file is needed for one of the integration tests

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `94f86eed95c2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
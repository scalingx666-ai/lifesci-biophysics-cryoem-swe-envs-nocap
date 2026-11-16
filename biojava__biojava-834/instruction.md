# Issue

**Title:** Uniprot short name fix

## Description

Update UniprotProxySequenceReader.java

2 fixes:

1. Full Uniprot downloads contain spaces in some sequences, which cause a crash in setContents because space isn't a valid compound. I added a replaceAll to strip out the whitespace.
2. Apparently a shortName element can be repeated. In both cases where we can encounter a shortName element, get the list (perhaps of length 0 or 1) and iterate over each shortName instead of just grabbing the first one.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `323cd9f01f59`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
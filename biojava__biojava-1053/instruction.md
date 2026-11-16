# Issue

**Title:** Upgrading source and target to java 11

## Description

Needed for the 7.0.0 release as discussed in #1035 

After this, the minimum required JRE to run biojava (or projects that use it) is JRE 11

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `99aae0415ab5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
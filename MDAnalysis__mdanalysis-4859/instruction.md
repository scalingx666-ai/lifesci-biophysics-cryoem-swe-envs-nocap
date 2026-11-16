# Issue

**Title:** [fmt] Format everything but package and testsuite

## Description

Formatting of everything excluded from `package` and `testsuite`. I did not add a `.black` configuration file for these files that are not touched often, but will probably do at the very end of the whole process.

<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--4859.org.readthedocs.build/en/4859/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9110a6efe276`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
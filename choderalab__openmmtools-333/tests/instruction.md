# Issue

**Title:** Add option to restrain only oxygens

## Description

Addresses JDC's request
(https://choderalab.slack.com/archives/C0LD6795X/p1518393073000045) to
provide an option that allows the origin-centered harmonic restraining
potential to apply only to oxygens, rather than all atoms

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `87ebee6a2bf3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
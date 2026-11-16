# Issue

**Title:** Output more detailed messages when template matching fails

## Description

Here's an attempt at generating more informative errors when a residue in a topology doesn't match any template in a force field.  Addresses #1195 and #3181.  The test cases should give some ideas about the sorts of messages generated.  I've additionally tested it manually on many of the relevant examples that have been submitted to the issue tracker over the years.

Essentially, differences between the set of heavy atoms is checked first, then all particles, then internal bonds, then external bonds.  If a template is found where all atoms and bonds match (based on their elements), only then is a generic error message returned indicating that the connectivity is different.  Special messages are shown for common cases like the presence of an element not supported by any template, atoms matching exactly but all bonds missing, missing H atoms or extra particles, and terminal capping problems.

Any feedback is welcome as to other special cases we can (easily) check or changes to what should be reported.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b619675415da`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
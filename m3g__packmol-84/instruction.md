# Issue

**Title:** read repeated connectivity and print in the same CONECT line

## Description

With this PR, repeated connectivity lines for the same atom are accumulated and the connectivity is not lost, but printed in the same output line. For instance, this connectivity:

```
CONECT    1    2    6    7
CONECT    1    2
CONECT    2    1    3    8
CONECT    2    1
```

will be printed in two lines, as:

```
CONECT    1    2    6    7    2
CONECT    2    1    3    8    1
```

This aims to fix  .

To do:

- Check if this is an acceptable solution.
- Add tests.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d7585c763369`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
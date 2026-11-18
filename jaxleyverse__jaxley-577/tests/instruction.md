# Issue

**Title:** Prepare release; allow easy access of version

## Description

I realized that:
```python
import jaxley as jx
print(jx.__version__)
```
had not been possible due to the missing import.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `15040d616f1d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Import all modules starting with pyiron_*

## Description

```
from pyiron_base import Project
pr = Project(".")
print(dir(pr.job_type))
import pyiron
print(dir(pr.job_type))
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d6d869ff416d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
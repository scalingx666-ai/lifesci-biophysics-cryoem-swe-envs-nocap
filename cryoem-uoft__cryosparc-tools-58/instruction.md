# Issue

**Title:** Docs: show warning when connecting to incompatible CryoSPARC

## Description

Example output:
```
In [1]: from cryosparc.tools import CryoSPARC

In [2]: cs = CryoSPARC(base_port=62000)
<ipython-input-2-3c3de64e7601>:1: UserWarning: CryoSPARC instance localhost:62000 with version v4.4.0 may not be compatible with current cryosparc-tools version v4.3.1.

To install a compatible version of cryosparc-tools:

    pip install --force cryosparc-tools~=4.4.0

Or, if running a CryoSPARC pre-release or private beta:

    pip install --force https://github.com/cryoem-uoft/cryosparc-tools/archive/refs/heads/develop.zip

  cs = CryoSPARC(base_port=62000)

In [3]:
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `29913c2be454`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Disable TF32 usage by default

## Description

Tensornet enables tf32 by default. We find these affect the accuracy too greatly in some situations so I am disabling them out of caution (which is the default in pytorch).
User code can request TF32 usage by writing this after importing torch:

```python
torch.set_float32_matmul_precision("high")
torch.backends.cuda.matmul.allow_tf32 = True
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c9d55fc02914`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
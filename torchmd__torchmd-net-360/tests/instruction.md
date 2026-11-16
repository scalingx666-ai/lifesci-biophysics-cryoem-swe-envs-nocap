# Issue

**Title:** Update setup.py to check if WITH_CUDA is set

## Description

`os.environ.get("WITH_CUDA", "0") == "0"` return `True` even if the environment variable is not set. Thus currently, the default behavior is to automatically fall back to CPU-only build if `WITH_CUDA` is not set. 

This introduces a small change to the first check if the `WITH_CUDA` is set. If not set, it falls back to the build defined by `torch.cuda._is_compiled()`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b4772e259267`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
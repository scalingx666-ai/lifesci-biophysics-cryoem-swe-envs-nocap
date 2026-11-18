# Issue

**Title:** feat: add crackle parameter to download

## Description

Super low RAM image download.

```python
arr = cv.download(..., crackle=True) # arr is a CrackleArray
```

Here's a RAM chart vs renumber=True. Without renumber=True, the image size was 16 GB. It's possible to do even better by using renumber=True internally, but crackle's remap/astype functions need improvements to support this.

<img width="840" height="464" alt="image" src="https://github.com/user-attachments/assets/f49df0a6-4035-4054-a2e6-ed4a2641fdc9" />

(black) renumber=True (blue) crackle=True

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `355e58c37200`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
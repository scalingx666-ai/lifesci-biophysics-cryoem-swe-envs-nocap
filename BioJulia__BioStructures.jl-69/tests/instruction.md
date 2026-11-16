# Issue

**Title:** Add error hint for extension packages

## Description

This provides a hint to the user about how to fix MethodErrors.

Before:
![image](https://github.com/user-attachments/assets/76da919b-c2b4-442e-aaef-30654c9f1a89)

After:
![image](https://github.com/user-attachments/assets/0da3d84f-7466-40e5-9fd0-d702edb24e30)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b18bec3f3f7d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
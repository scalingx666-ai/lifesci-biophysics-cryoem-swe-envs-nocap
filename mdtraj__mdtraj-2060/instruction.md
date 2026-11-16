# Issue

**Title:** Minor bug fix for documentation equations

## Description

Fix for equation rendering on the documentation page for [`asphericity`](https://mdtraj.readthedocs.io/en/latest/api/generated/mdtraj.asphericity.html) and [`relative_shape_anisotropy`](https://mdtraj.readthedocs.io/en/latest/api/generated/mdtraj.relative_shape_antisotropy.html) in `mdtraj/mdtraj/geometry`.

<img width="759" height="327" alt="Screenshot 2025-07-23 at 1 34 44 PM" src="https://github.com/user-attachments/assets/d65f2052-67ba-4342-993d-877454582936" />
<img width="742" height="353" alt="Screenshot 2025-07-23 at 1 34 49 PM" src="https://github.com/user-attachments/assets/77d2ad2a-8d88-4b45-a6f3-1572e92fbe4e" />

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `99f60b649ae1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
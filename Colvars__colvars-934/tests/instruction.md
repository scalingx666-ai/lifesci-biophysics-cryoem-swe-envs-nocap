# Issue

**Title:** perf: fix performance regression when using CUDAGM with CPU Colvars

## Description

It looks like the vector with custom allocator is copied instead of moved in the original code of cvm::atom_group::positions_shifted. This commit tries to fix the issue by avoiding any potential constructing and destroying new vectors in positions_shifted.

Before this fix, nsys-ui shows:
<img width="886" height="97" alt="屏幕截图_20260518_155252" src="https://github.com/user-attachments/assets/79288e05-7584-4c93-a8aa-1035434f2117" />

After this fix, nsys-ui shows:
<img width="886" height="97" alt="屏幕截图_20260518_155204" src="https://github.com/user-attachments/assets/25c97cc8-b1ee-4c29-9e39-37a2aeacaa92" />

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `280943ea1020`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
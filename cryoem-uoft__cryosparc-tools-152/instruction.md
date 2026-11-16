# Issue

**Title:** feat: setting job output and tile images

## Description

- New `job.set_output_image` function
- New `job.set_tile_image` function
- New `image=` argument for `job.save_output`, 
- New `image=` argument for `cs.save_external_result` and associated `project` and `workspace` aliases

<img width="940" height="247" alt="Custom tile image examples." src="https://github.com/user-attachments/assets/4c8d2d96-0e6e-49bb-9ad1-3743d61091ea" />
<img width="672" height="651" alt="Custom output image examples." src="https://github.com/user-attachments/assets/d46ea40a-ce68-4192-8207-314c33cb7ba3" />

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `91d8c315bcb7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
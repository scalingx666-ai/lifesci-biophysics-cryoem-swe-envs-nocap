# Issue

**Title:** feat(bbox): handle plain lists of 6 ints in Bbox.create

## Description

When the input to Bbox.create is a plain list or tuple of 6 integers, use Bbox.from_list instead of from_slices for more intuitive handling.

This makes it easier to create a bounding box from coordinates in the format [x_min, y_min, z_min, x_max, y_max, z_max].

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `46745da3ecd2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
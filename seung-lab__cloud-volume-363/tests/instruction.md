# Issue

**Title:** fix(encode_jpeg): Neuroglancer expects vertical tile layout

## Description

Don't exactly understand the operations involved here, but I verified that with the change below a chunk downloaded with CloudVolume and re-uploaded as JPEG shows up at the correct position / in the correct orientation in Neuroglancer. Previously, for my 128x128x16 chunks Neuroglancer would complain about shape 16384x16, instead of the expected 128x2048

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0bb444f2414c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
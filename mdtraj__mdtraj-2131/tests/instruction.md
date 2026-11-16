# Issue

**Title:** Fix max_face_to_face_centroid_distance value

## Description

max_face_to_face_centroid_distance was set in units of Angstrom rather than nm. Corrected from 5.5 to 0.55 in accordance with the distance used for FaceToFace(BasePiStacking) in ProLIF.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2f4b592fc96a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
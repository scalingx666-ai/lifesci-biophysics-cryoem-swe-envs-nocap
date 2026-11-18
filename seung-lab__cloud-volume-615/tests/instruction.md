# Issue

**Title:** BREAKING: feat: add support for jpegxl image compression + make some codecs optional

## Description

This works!

This change also makes many compression codecs optional.

base set: jpeg, compressed_segmentation, gzip, brotli

extras: fpzip, kempressed, zfpc, compresso, crackle, png, blosc, jpegxl

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `84736a77149d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
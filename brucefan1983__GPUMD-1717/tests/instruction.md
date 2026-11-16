# Issue

**Title:** Supports specifying atomic mass and solve limits on group keywords

## Description

**Summary** (Briefly summarize the purpose of this pull request)
Fixing the error where the deposition function cannot customize atomic mass and recognition group information

**Modification** (Describe the main changes made in this pull request)
Optional mass information has been added to both the “atom” and “file” modes.
Example:
atom mode : `H 10 -0.01 2`
file mode : `H 0 0 100 0 0 -0.01 2`

**Licensing** (Please read and confirm before submitting this pull request)

By submitting this pull request, I agree that my contribution will be included in GPUMD and redistributed under the existing GPUMD license. Newly added source files follow the existing GPUMD license-header style when applicable. No external source code with incompatible licensing has been copied into this pull request.

**Validation** (Describe how this pull request was tested)

**Others**

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0d41977c4465`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
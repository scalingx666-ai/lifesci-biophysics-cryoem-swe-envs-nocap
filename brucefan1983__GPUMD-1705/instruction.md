# Issue

**Title:** fix fine_tune and change the access pattern of descriptor parameters internally

## Description

# **Summary**

Improve the internal access pattern of descriptor parameters and fix NEP89 fine-tuning.

# **Modification**

- Store descriptor parameters in channel-major order internally while preserving the existing `nep.txt` and `nep.restart` formats.
- Update all related NEP training paths.
- Add dedicated compatibility checks for fine-tuning.
- Update the documentation.

# **Validation**

Standard training, restart, and NEP89 fine-tuning were tested.

# **Licensing** (Please read and confirm before submitting this pull request)

By submitting this pull request, I agree that my contribution will be included in GPUMD and redistributed under the existing GPUMD license. Newly added source files follow the existing GPUMD license-header style when applicable. No external source code with incompatible licensing has been copied into this pull request.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6cbb1b2a36aa`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** reorder nep cij parameters

## Description

# **Summary** (Briefly summarize the purpose of this pull request)

Reorder $c_{ij}$ parameters such that the reading is faster. 

# **Modification** (Describe the main changes made in this pull request)

Reorder the descriptor parameters after reading them from the potential model file.

# **Licensing** (Please read and confirm before submitting this pull request)

By submitting this pull request, I agree that my contribution will be included in GPUMD and redistributed under the existing GPUMD license. Newly added source files follow the existing GPUMD license-header style when applicable. No external source code with incompatible licensing has been copied into this pull request.

# **Validation** (Describe how this pull request was tested)

The refactor does not change any existing results. 

# **Others**

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3fccd22b3861`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
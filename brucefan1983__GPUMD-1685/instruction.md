# Issue

**Title:** simplify interface

## Description

**Summary** (Briefly summarize the purpose of this pull request)

Simplify the `Property` callback interface.

**Modification** (Describe the main changes made in this pull request)

Provide default no-op implementations for optional callbacks in `Property`, so derived classes only need to override the callbacks they use.

Remove the now-unnecessary empty `process()` and `postprocess()` implementations from `Deform`.

No functional behavior is changed.

**Licensing** (Please read and confirm before submitting this pull request)

By submitting this pull request, I agree that my contribution will be included in GPUMD and redistributed under the existing GPUMD license. Newly added source files follow the existing GPUMD license-header style when applicable. No external source code with incompatible licensing has been copied into this pull request.

**Validation** (Describe how this pull request was tested)

**Others**

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8df832fc63f4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
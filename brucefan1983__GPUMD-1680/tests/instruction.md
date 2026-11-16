# Issue

**Title:** fix npt_scr rotation problem

## Description

**Summary** (Briefly summarize the purpose of this pull request)

Fixes two problems in NPT-SCR:
* random rotation for triclinic box
* slightly different scaling for different directions for orthogonal box

**Modification** (Describe the main changes made in this pull request)

**Licensing** (Please read and confirm before submitting this pull request)

By submitting this pull request, I agree that my contribution will be included in GPUMD and redistributed under the existing GPUMD license. Newly added source files follow the existing GPUMD license-header style when applicable. No external source code with incompatible licensing has been copied into this pull request.

**Validation** (Describe how this pull request was tested)

<img width="1580" height="977" alt="image" src="https://github.com/user-attachments/assets/f3c1c1a6-9410-4393-96e8-6bb574db4b6c" />

<img width="790" height="490" alt="image" src="https://github.com/user-attachments/assets/c77b6ce1-e9c3-4663-87a4-bf86ab117a25" />

<img width="790" height="490" alt="image" src="https://github.com/user-attachments/assets/d7f450d4-4256-44dd-a43e-b25276349b58" />


**Others**

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f8cc14ac8d48`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
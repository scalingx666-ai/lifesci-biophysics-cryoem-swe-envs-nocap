# Issue

**Title:** Alchemical transformations

## Description

This PR is a work in progress addressing issue 

Disclaimer: due to lack of time, I have heavily relied on VSCode Copilot to implement the new functions. The result is a bit ugly, but appears to be working for now. There is definitely something odd with the `rebuild` command: the template files were correctly created, yet only the rendered `Data Atoms` file would not be updated, while the Bonds, Angles, Dihedrals, and Impropers files would show updated types for the terms including atoms whose type has been changed. This problem has been patched with some ugly checks done by the `rebuild` command, but I believe that a more elegant solution can be found. When I have the time to properly review the code, I may also find a better solution.

I am attaching a revised test to demonstrate the use of the commands, plus some tests for comparison.
[alchemical.tar.gz](https://github.com/user-attachments/files/23698283/alchemical.tar.gz)

Please test this version on your samples, and report any change you may observe. Thanks!

>**EDIT**:
> The original PR and the attached example are based on new commands. This approach has been replaced with a more logical one, suggested by Andrew. The original commit for the original pull request is [8082d64](https://github.com/hothello/moltemplate/tree/8082d64d898fa682fda3f1010800bf7a7c6d5874)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `031ecd4efc7a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Throw error for "explicit" hydrogens that aren't in molecule graph

## Description

This PR flags with an error implicit hydrogens missed by RDKit, e.g., [NH+], as referenced in:
 - Issues resolved:
    - 
    - 
 - Resolves QCA-Dataset Submission 
 - This PR might partially be related to partially address QCS-Dataset-Submission 
- This PR seeks to throw an error for unacceptable CMILES strings, why these strings are created is a topic of OpenFF-Toolkit , QCSubmit  and , and OpenFF-Sage . Possible causes could be related to OpenFF-Toolkit .

 - The solution contributed in this PR throw appropriate errors for molecules in the QCA-Dataset Submissions which were erroneously submitted: ,  as outlined in OpenFF-Toolkit  (resolved in this PR)

- [X] Tag issue being addressed
- [X] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [X] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://open-forcefield-toolkit.readthedocs.io/en/latest/developing.html#style-guide) codebase
- [x] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a92402f9aab9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** [PATCH] added line truncation to pdb_tidy for >80 chars

## Description

Stated behaviour of pdb_tidy:

```
Modifies the file to adhere (as much as possible) to the format specifications.


Expects a sorted file - REMARK/ATOM/HETATM/END - so use pdb_sort in case you are
not sure.

This includes:
    - Adding TER statements after chain breaks/changes
    - **Truncating**/Padding all lines to 80 characters
    - Adds END statement at the end of the file

Will remove all original TER/END statements from the file.

Usage:
    python pdb_tidy.py [-strict] <pdb file>

Example:
    python pdb_tidy.py 1CTF.pdb
    python pdb_tidy.py -strict 1CTF.pdb  # does not add TER on chain breaks
```
    
**However** truncating lines >80 characters is not implemented in the code.
I have added this functionality and a corresponding unit test.

Current behaviour

**INPUT**
```
TITLE     THIS IS A VERY LONG LINE WHICH SHOULD BE WRAPPED CORRECTLY OVER MULTIPLE LINES WITH PADDING\n
```
**OUTPUT** _line remains unchanged_

Desired behaviour

**INPUT**
```
TITLE     THIS IS A VERY LONG LINE WHICH SHOULD BE WRAPPED CORRECTLY OVER MULTIPLE LINES WITH PADDING\n
```
**OUTPUT**
```
TITLE     THIS IS A VERY LONG LINE WHICH SHOULD BE WRAPPED CORRECTLY OVER       \n
TITLE     MULTIPLE LINES WITH PADDING                                           \n
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f087b488a756`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
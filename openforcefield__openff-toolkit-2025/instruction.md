# Issue

**Title:** Speed up identical molecule detection

## Description

- [x] Addresses #2008 
- [x] Document change in Molecule.ordered_connection_table_hash
- [ ] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [x] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://docs.openforcefield.org/projects/toolkit/en/stable/users/developing.html#style-guide) codebase
- [x] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

Using the excellent reproducing code snippet from #2008 (though restricting mol size to 10^3 instead of 10^4) we see about a 50x speedup


Before
<img width="585" alt="Screenshot 2025-02-23 at 1 32 11 PM" src="https://github.com/user-attachments/assets/7d62ccef-1ef9-41c8-8426-94645611eaa1" />


After 
<img width="595" alt="Screenshot 2025-02-23 at 1 31 53 PM" src="https://github.com/user-attachments/assets/a78abe60-693d-4486-be63-822af363fd88" />

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f4fff548c856`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
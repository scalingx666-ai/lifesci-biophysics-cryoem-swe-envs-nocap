# Issue

**Title:** pdb_mkensemble now opens file by file

## Description

Attempting to use `pdb_mkensemble` in 3000 files output the following error during `.check_input()`:

```
Traceback (most recent call last):
  File "/home/joao/Programming/pdb-tools/bin/pdb_mkensemble.py", line 134, in <module>
  File "/home/joao/Programming/pdb-tools/bin/pdb_mkensemble.py", line 108, in main
  File "/home/joao/Programming/pdb-tools/bin/pdb_mkensemble.py", line 58, in check_input
OSError: [Errno 24] Too many open files: '_01021.pdb'
```

`pdb_mkensemble` was opening pdb files during the `.check_input()` call, overloading memory.

After discussion with @JoaoRodrigues open was moved to `make_ensemble()` and `check_input()` now returns `args`: a list of file names.

_PR passed tests and made ensemble of 3000 frames correctly in my system._
```
----------------------------------------------------------------------
Ran 241 tests in 0.142s

OK
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `abdbb060743f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
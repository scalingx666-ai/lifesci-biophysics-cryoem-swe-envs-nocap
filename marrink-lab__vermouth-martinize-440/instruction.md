# Issue

**Title:** Fix/424

## Description

Fix #439 
Note that the tests currently fail, since the behaviour of the parser regarding the replace attribute has changed. The pytest traceback is not pretty, but instead of using the "new" atomnames (after modification) we'd need to use the "old" ones. An example of this comes from one of the test forcefields (vermouth/tests/data/force_fields) pepplane. Instead of:
```
[ modification ]
[ from ]
universal-test
[ to ]
pepplane
[ from blocks ]
C-ter
[ to blocks ]
C-ter
[ mapping ]
C   BB
O1  BB
O2  BB
[ reference atoms ]
BB C
```
we'd have to write
```
[ modification ]
[ from ]
universal-test
[ to ]
pepplane
[ from blocks ]
C-ter
[ to blocks ]
C-ter
[ mapping ]
C   BB
O  BB
OXT  BB
[ reference atoms ]
BB C
```
See also: `vermouth/tests/data/force_fields/universal-test/modifications.ff` and `vermouth/tests/data/mappings/universal-test/pepplane/modifications.mapping`.
@fgrunewald Is this change in behaviour acceptable/desirable? Or should it be augmented so you could use either name?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e6f16e12831c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
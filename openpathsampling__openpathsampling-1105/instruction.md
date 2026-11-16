# Issue

**Title:** Add `restart_at_step` for `ExternalEngine` (for numeric filenames)

## Description

with #1103 and #1102, I believe this implements the last thing discussed in  #1101:

It adds a `restart_at_step` for `ExternalEngine`s, which (if `self.filename_setter` is `FilenameSetter`) set's it's count to the last written trajectory + 1

this will error out if it tries to read filenames that don't end in a number, for now a `ValueError: invalid literal for int() with base 10: ''`, but this can be updated to make it more clear.

Also updated the AD gromacs example to include the required restart code.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f1e89610cb67`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
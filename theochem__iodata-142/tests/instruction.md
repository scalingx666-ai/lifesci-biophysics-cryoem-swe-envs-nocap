# Issue

**Title:** Make load_many XYZ work with terminal blank lines

## Description

I have noticed that if there are empty lines at the end of an extended/concatenated XYZ file format, it will fail with a `ValueError`. I understand that there should be no blank lines at the end of the file, but extended XYZ is a loosely defined format and one can easily come across such files. So, I fixed this and added a new test and example of an extended XYZ file of a set of molecules. Please feel free to share any comments you might have.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `52185fdf2250`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
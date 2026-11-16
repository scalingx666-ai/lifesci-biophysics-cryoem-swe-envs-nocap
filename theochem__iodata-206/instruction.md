# Issue

**Title:** Fix Failure of Parsing PDB wihout an END

## Description

I am not sure how common (or even legitimate) this issue is, but I have been encountering PDB files that don't have an `END`, so trying to load them fails... I can investigate more to see exactly when such PDB files are found, but I think it was when using Biopython to manipulate a PDB file...

I changed the `load_one` to print a warning message if `END` is not found, and still return the data. I have attached an example, but didn't add it to the test files, because I wasn't sure it is worth it. We can remove the `END` of one of the existing pdb files to test this, probably.
[1JBEA.pdb.tar.gz](https://github.com/theochem/iodata/files/5144873/1JBEA.pdb.tar.gz)

Please feel free to let me know what you think.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ccda7878990a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
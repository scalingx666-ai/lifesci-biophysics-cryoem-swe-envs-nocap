# Issue

**Title:** Fix #934 Several file and path issues on windows

## Description

MS Windows has file and path problems:
1- spaces are not handled well in URL. Converting it to a URI handles decoding the path well.
2- TestURLIdentifier fails because when the connection is to a file and it contains parameters (e.g. ?format=PDB), they are perceived as part of the file name. getting the path first fixes it.
3- TestMmtfStructureReader and TestMmtfStructureWriter fail because Path does not handle drive specifier with a colon ':' (e.g. C:) well, while File does.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7b76f8f47a09`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
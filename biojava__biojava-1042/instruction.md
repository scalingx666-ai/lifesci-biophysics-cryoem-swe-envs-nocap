# Issue

**Title:** GenBankWriter does not use information in the original header #942

## Description

Hello, 
I have been working on the issue that a GenBank file read in through the parser and then written back out will result in loss of information from the original file. While looking into this I have also fixed a few other issues with the parser and writer for Genbank files. The highlights of this work includes:
- added a new writer helper method that will retain the information in the original locus line
- fixed writing out db_xref feature qualifiers which was not working
- fixed writing out the accession version and GI ID
- allow locus ids that contain spaces
- allow the locus id to be empty
- fixed writing out feature locations containing joins and partial locations
- fixed reading and writing locations on multiple lines
- Maintained quotes around feature qualifier values in files that are written out
- Added a test and test file for feature locations containing joins
- Added a test to check for differences between an original file and one that had been written out by the writer

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3b90e2b4d779`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
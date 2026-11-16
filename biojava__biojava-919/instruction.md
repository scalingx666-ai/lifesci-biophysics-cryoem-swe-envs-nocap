# Issue

**Title:** Github issue 843 - Genebank parser

## Description

#### Reference Issue
Partly  

#### What does this implement/fix? Explain your changes.
- Corrected GenbankReader.java to parse anticodon qualifiers when they line wrap.
- Wrote new testGithub843 in GenbankReaderTest.java
- Added mentioned file NC_018080.gb downloaded from https://www.ncbi.nlm.nih.gov/nuccore/NC_018080

#### Additional comments
@josemduarte , can you please tell me where I can get NC_018080 in .embl file format? I couldn't find it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `72eae71bb33d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
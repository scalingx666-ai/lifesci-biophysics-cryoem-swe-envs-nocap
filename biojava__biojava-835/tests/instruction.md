# Issue

**Title:** Fix regex used in Genbank LOCUS line parsing

## Description

Hi,

This is my attempt to resolve issue #833 that will bring biojava 5 inline with legacy biojava in allowing uppercase units, strand and topology strings in the locus line of genbank files.

I have also added a number of test genbank files containing uppercase units, strands and typologies and a test to ensure the GenbankSequenceParser can handle these files that were previously throwing a ParserException("Bad locus line").

This is my first go at a pull request for biojava so please let me know if I have done anything wrong.

Thanks,
James

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `323cd9f01f59`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
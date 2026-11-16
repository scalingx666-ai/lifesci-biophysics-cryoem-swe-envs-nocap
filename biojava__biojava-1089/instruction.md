# Issue

**Title:** Fix container inefficiency in GenericFastaHeaderParser.java

## Description

Hi,

We find that there exist container inefficiency in GenericFastaHeaderParser.java.

In particular, at lines 96 and 97 in GenericFastaHeaderParser.java, the for loop keep allocating the `data` array and transform container object `values` to array, which incurs redundant memory allocation and container operations.

We discovered the above containers inefficiencies by our tool cinst. The patch is submitted. Could you please check and accept it? We have tested the patch on our PC. The patched program works well.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `266d835be823`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
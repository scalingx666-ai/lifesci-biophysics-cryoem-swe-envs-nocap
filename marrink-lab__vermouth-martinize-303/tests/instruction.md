# Issue

**Title:** Fix for DNA termini modifications

## Description

Paulo found an issue when he tried to make DNA termini modifications. In particular, these modifications did not add any PTM_atoms, breaking several assumptions.
The first commit makes sure that any CLI annotations regarding modifications are picked up by canonicalize_modifications, and later on do_mapping.
The second commit fixes an open TODO in the do_mapping processor, so that the modifications attribute now is a part of the matching criteria. Otherwise I'd have modification mappings applying multiple times.

This is still a draft, since the second commit broke some tests in do_mapping. I'm not sure yet this is harmless, nor how to fix them.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ccc3890c249d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
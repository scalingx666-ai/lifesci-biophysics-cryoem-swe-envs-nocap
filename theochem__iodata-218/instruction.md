# Issue

**Title:** PDB format: Load & Dump Chain ID

## Description

I was using `IOData` to load some `PDB` files, and needed the chain ID information but noticed that:

- `load_one` does not read chain ID
- `dump_one` uses `A` as a default for everything

So, I fixed these to parse chain id (if available), and when dumping a pdf file leave chain id empty if not available. I also polished the exiting code a bit to put the information parsed in the order they appear on a PDB ATOM line. 

Please feel free to let me know if anything needs to be undone.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `af86c3c0ce43`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Secstruc gmx header fix

## Description

Per #689 I thought I had something weird going on with an mdtraj installation not being picked up and used, but it turned out that was a local environment problem. While tracking it down to make sure it was all ok, I did notice that with the recent changes 

1) the dssp string wasn't actually being written to the header because we were still looking for a `secstruct` attribute not `aasecstruct`
2) the business with changing the effective string wrt IDRs wasn't actually being used because of where the dssp gmx header annotation comes in the pipeline.

So, this PR fixes 1) easily and 2) by moving some code around

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `728a38b8d6e8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
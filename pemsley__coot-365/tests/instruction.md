# Issue

**Title:** Add Altona-Sundaralingam ribose pucker calculation

## Description

This PR Adds the Altona-Sundaralingam ribose pucker calculation to the get_pucker_analysis_info function. 

New fields in JSON
- P_deg - the pseudorotation angle
- pucker_name - the name of the conformation (e.g. C2-exo, C3-endo)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a7541c3541e8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
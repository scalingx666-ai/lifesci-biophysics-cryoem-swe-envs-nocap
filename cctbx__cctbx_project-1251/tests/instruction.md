# Issue

**Title:** probe2/clashscore2: summarize contacts per atom pair instead of keeping every dot

## Description

clashscore2 had probe2 write every dot to JSON and parsed it back. This adds
`ContactDB` (C++), which runs probe2's per-atom loop with the same filters and
`DotScorer::check_dots()`, but keeps for each source/cause pair the dot count
per interaction type, the smallest gap and the smallest-gap Bump. With
`output.contact_records=True` probe2 keeps those instead of dots, and
clashscore2 builds its clash list from them. clashscore2 now uses this by
default (`probe_clashscore_manager.use_contact_records`); `mmtbx.probe2`
output is unchanged unless the option is set.

probe2 and clashscore2 also skip the plain-pair table (read only by ADP
restraints) when they process a model just for its bonds, as place_hydrogens
now does (23df24b570).

On a large structure (1aon) this cuts clashscore2's peak memory by roughly a
quarter. `tst_probe2_contact_records.py` checks the clash list and clashscore
against the per-dot JSON path. Depends on #1249.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b26661e2d0ac`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
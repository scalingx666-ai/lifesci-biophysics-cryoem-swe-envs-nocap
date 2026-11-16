# Issue

**Title:** Corrects behavior with `ANISOU` lines in `pdb_selaltloc`

## Description

Some time ago, #117 improved `pdb_selaltloc` considerably. Still some cases referring to the `ANISOU` lines were not covered by the tests which resulted in some bugs -> #130.

From #130, I noticed `pdb_selaltloc` was also not working properly with residue `VU7`.

**Done in this PR:**
- Solved #130 problem with `ANISOU` lines
- Solved problem with `VU7` (`7K2U.pdb`) like cases found when exploring #130
- all previous tests pass without modification
- added eight more tests

@JoaoRodrigues to my understanding, `ANISOU` lines do **not** have information on the `occ`. Do they? If yes, where?

Should be merged as `PATCH`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `10a7a374fb9e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
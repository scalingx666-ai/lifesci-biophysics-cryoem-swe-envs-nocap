# Issue

**Title:** Write the effective core charge in the WFN dump

## Description

The WFN writer put the atomic number in the `CHARGE` field of every atom. In a WFN file this field is the effective nuclear charge, so ghost atoms (core charge 0) and atoms with an ECP got the wrong value. The WFX writer already uses `atcorenums`, so this makes WFN match.

I found it by loading every test file, dumping it to the other formats and loading it again. `he2_ghost_psi4_1.0.molden` has `atcorenums = [0, 2]`. After a trip through WFN the charge of the system changed from 0 to 2, because the file said `CHARGE = 2.0` for the ghost atom.

The change is only in `dump_one`: it writes `data.atcorenums` instead of `data.atnums` in the charge column, and `atcorenums` is listed as an optional field in the docs. When `atcorenums` is not set it falls back to the atomic numbers, so nothing changes for normal files. The loader still builds `atnums` from the element symbol and does not read the charge column back. I left that alone because `load_wfn_low` is tested directly.

Added `test_dump_one_atcorenums`, which sets `atcorenums = [6, 1, 0]` on the water test file and checks the charges in the output. It fails without the fix (it writes 8.0 where 6.0 is expected). The wfn, wfx and api tests pass.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `10c71d590918`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
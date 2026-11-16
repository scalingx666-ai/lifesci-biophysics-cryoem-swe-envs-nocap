# Issue

**Title:** Name each molecule's undo backups by imol, not "placeholder"

## Description

## The bug

In libcootapi, every `molecule_t` writes its undo backups to `coot-backup/<mol_name>-<n>.pdb`. `modification_info.mol_name` defaults to `"placeholder"` and is never set anywhere in `api/`, so all molecules share the same file names. With two models edited, the second molecule's backups overwrite the first's, and `undo()` on the first molecule loads the second molecule's coordinates.

Reproduced with chapi from CCP4 (`coot_headless_api`) and with Moorhen's WASM build:

| step | molecule A (5a3h) | molecule B (tm-A) |
|---|---|---|
| loaded | 2765 atoms | 720 atoms |
| delete a residue in each | 2758 | 712 |
| `undo(A)` | **720**, now a copy of B | 712 |

## Fix

Set `modification_info.mol_name = "imol-" + std::to_string(imol_no)` in `molecule_t::init()`. Every constructor sets `imol_no` before calling `init()`. New molecules get `molecules.size()` as their index, so it is unique within the container and never reused. `copy_molecule()` builds a fresh `molecule_t` too.

Moorhen already ships this as a build patch (moorhen-coot/Moorhen#718), with a regression test that edits two molecules and undoes each. The test fails without the change and passes with it, and the rest of Moorhen's API test suite passes. I haven't built or tested this natively against current `main`, but the change applies cleanly and the backup code hasn't changed since April 2024.

## Not changed here

`molecules_container_t::set_make_backups()` stores `make_backups_flag`, but nothing reads it, so it has no effect. Backups are controlled per molecule by `really_do_backups` instead.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9648092d258f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
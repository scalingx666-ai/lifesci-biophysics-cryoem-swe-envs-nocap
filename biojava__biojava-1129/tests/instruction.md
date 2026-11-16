# Issue

**Title:** Bugfix: parsing of mmCIF files with empty database_PDB_rev.date

## Description

Files with a block like the following were failing parse with a DateTimeParseException:
```
#
_database_PDB_rev.num             .
_database_PDB_rev.date            ?
_database_PDB_rev.date_original   2026-07-14
_database_PDB_rev.status          ?
_database_PDB_rev.replaces        ?
_database_PDB_rev.mod_type        ?
#
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e3d58c033f68`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
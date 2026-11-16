# Issue

**Title:** replace doc footer

## Description

- fix #383
- replaced explicit (incomplete) author list in doc footer with The alchemlyb development team and a link to AUTHORS
- added @xiki-tempula to the top of AUTHORS (which brings the author file in line with the author list and acknowledgements of the JOSS paper PR #328 )

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `124f14846211`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
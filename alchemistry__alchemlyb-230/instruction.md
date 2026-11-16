# Issue

**Title:** Removed extraction of dHdl avg and components in AMBER parser

## Description

Addressing issue #226, I removed the useless extraction of dHdl averages (and dHdl components at the same time).

PS: I also made a really small adjustment to the ```skip_lines``` method of ```SectionParser```, as it stated it skipped a given number of "files" (????) while it should obviously be "lines" :-)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `21532bdc529c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
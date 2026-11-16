# Issue

**Title:** Use MDTraj codecs in SimStore JSON serialization

## Description

Well this is an embarrassing bugfix to have missed for so long. @sroet, could you spare 30 seconds to review my fix of this facepalm?

I will merge this immediately after review or after it sits open for over 24 hours, merging around Tue 02 Aug 23:00 GMT (18:00 my local).

@ShenWenHuibit: This should fix the current problem you're having. If you can do a developer install using the fork `dwhswenson` and branch `use-mdtraj-codec`, you should be able to try it out immediately.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e56618636ae6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
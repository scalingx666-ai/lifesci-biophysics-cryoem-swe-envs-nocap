# Issue

**Title:** [v2.9] Stop with an error when two actions write the same file

## Description

When two actions opened the same output file (e.g. two METAD without FILE, both using HILLS) the file was silently corrupted: the second OFile::open backed up the file the first action was writing, so the first action continued on bck.0.HILLS, or, when restarting, the lines of the two actions were interleaved.

OFile::open now checks, before backing up the file, that no other open OFile linked to the same PlumedMain is writing on the same path. The same file is also detected when referred to with a different path (./FILE, absolute paths, links) by comparing device and inode numbers. /dev/null and non-regular files can still be shared, and input files are not considered (reading a file while writing it is allowed).

Two isdb regtests used the same HILLS files for two PBMETAD actions; they now use different names (hills are shared with WALKERS_MPI, so results are unchanged).

Claude-Session: https://claude.ai/code/session_016vwxtSkuGhjjgnjk4dCHfu

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8c4ccdc3b4f1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
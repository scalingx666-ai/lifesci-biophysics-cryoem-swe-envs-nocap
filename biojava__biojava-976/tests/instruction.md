# Issue

**Title:** A flexible system for passing custom URL layout in DownloadChemCompProvider

## Description

This makes it possible to point DownloadChemCompProvider to any custom CCD repository, with any layout. Related to recent additions to ftp.rcsb.org (see [news announcement](https://www.rcsb.org/news?year=2021&article=613b93b3ef055f03d1f222cf&feature=true) )

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7c12c61d666d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
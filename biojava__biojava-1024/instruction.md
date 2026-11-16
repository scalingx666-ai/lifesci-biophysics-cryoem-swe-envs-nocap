# Issue

**Title:** Add file download validation

## Description

When a file is being downloaded, two metadata files are created for size and hash (as possible).

I applied this validation framework to the files I know (structure files, SCOP installations, Ecod installation).
In case anybody knows something else where downloaded files can be validated, please advise.

I did not implement the hash code _at least yet_, because I don't know a place where a hash file is distributed with the resource file. In case anybody knows some example, please advise.

@sbliven was proposing to download to a temporary file before moving the load to the destination file. I found that `FileDownloadUtils.downloadFile()` does. Therefore, no need to do it ourselves. again, in case anybody knows a place which downloads files but does not use `FileDownloadUtils.downloadFile()` for download, please advise.

.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8267ca528770`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
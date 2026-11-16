# Issue

**Title:** Add option to generate log entries from .ff files

## Description

This PR adds the option to have martinize2 generate log statements based on .ff files. Blocks, Modifications and Links get debug, info, warning, and error sections, in which each line will become a log statement with the appropriate level.
This is still WIP, since I kind of want to be able to do e.g. 
```
[ info ]
Created a disulfide bridge between residues {SC1['resid']} and {>SC1['resid']}.
```
but I'm not quite sure yet how painful that's going to be (shouldn't be too bad though, `format` will do the heavy lifting). Does require some bookkeeping .

Question, should each line in a section result in a separate log message, or should these be aggregated to a single entry?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dc1ec652d788`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** AMBER parser now uses "ntpr" information properly (issue #221)

## Description

Here is a smaller PR in which just the parsing of time information in AMBER parsers is fixed, now both functions use ```ntpr``` information properly as for issue #221.

PS: I left also some minor changes (like removing an unused ```import os```) because I missed them while rolling back to the previous version, but are really really small and changes.

PS2: the commit history here is a bit messy, but looking at the files changed it should be clear the changes are minimum, and commits will be more structured in the future.

NB: the alchemtest PR I recently made is needed for this fix to succeed properly.

Hope now everything is fine @orbeckst  @xiki-tempula thank you for your time :-)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0d63a027e2e8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
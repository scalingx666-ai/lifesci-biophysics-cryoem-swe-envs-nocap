# Issue

**Title:** xfel.ui_server: fix root password handling on database init

## Description

PR #808 broke the interactive root-password path when initializing a new database. The password entered at the getpass prompt was discarded: a later line unconditionally overwrote rootpw with the db.server.root_password phil parameter, which is None in the interactive case, so ALTER USER ... IDENTIFIED BY '%s' % None set root's password to the literal string "None" instead of what the user typed.

The non-interactive path (db.server.root_password passed on the command line) kept working because of that same overwrite, but its intended branch assigned to a misspelled variable (rootw) and was dead code.

Fix the typo so the branch sets rootpw, and drop the redundant overwrite so the interactively entered password survives to ALTER USER.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `69256c2ad732`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** feat: additional command Project, Workspace and Job functions

## Description

Adds the following additional functions:

* Projects
  * create
  * move
  * attach/detach
  * archive/unarchive
  * delete
  * accept failed attach
  * set/clear default param
* Workspaces
  * delete
* Jobs
  * clone
  * link to/unlink from workspace
  * move to workspace
  * set params
  * clear param
  * set priority
  * replace connection
  * set/unset final
  * clone
  * delete

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1225d98d0ec4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
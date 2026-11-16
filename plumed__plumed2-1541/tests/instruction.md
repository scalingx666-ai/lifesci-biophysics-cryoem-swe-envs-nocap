# Issue

**Title:** Subprocess: signal the whole process group

## Description

Subprocesses are launched as "/bin/sh -c cmd", so signals sent to the child pid only reached the shell and not the processes it spawned (e.g. the python interpreter started by scripts/selector.sh). As a consequence stop()/cont() had no effect on the actual worker, and SIGKILL in the destructor left the grandchildren orphaned and skipped the shell EXIT traps (e.g. the removal of temporary files in selector.sh).

The child is now made leader of its own process group and all signals are sent to the group (kill(-pid,...)). Compared with the original proposal in #1126:

- setpgid is called in both parent and child, to avoid a race where the parent signals the group before the child has created it (e.g. GenericMolInfo calls stop() right after construction).
- the destructor sends SIGTERM followed by SIGCONT. SIGCONT is needed since the group might have been stopped, in which case a SIGTERM (or SIGINT) would remain pending and waitpid would hang forever.
- if the shell has not exited after a grace period of 1 s, the group is killed with SIGKILL. A final SIGKILL sweep also removes leftover processes in the group. The shell is not reaped (waitid with WNOWAIT) until the group has been signaled, so the pgid cannot be recycled.

A new regtest (rt-make-subprocess-signals) checks that, with PLUMED_ENABLE_SIGNALS set, grandchildren are terminated, EXIT traps are executed, a stopped group does not hang the destructor, and a process ignoring SIGTERM is killed anyway. The test fails on current master and hangs with the patch of #1126.

Based on #1126 by Tyson Whitehead.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `013e10a76734`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
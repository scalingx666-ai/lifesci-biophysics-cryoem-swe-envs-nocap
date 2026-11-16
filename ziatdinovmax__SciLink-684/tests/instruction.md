# Issue

**Title:** Generated scripts run in their own process group and are ended whole

## Description

## Summary

When an analysis script ran too long, or you pressed Stop, SciLink ended the script but not the programs the script had started itself: an external solver, a helper process, a worker pool. Those could keep running, holding memory and CPU after the analysis had moved on. Now the script and everything it started are ended together. Scripts that finish normally are unaffected.

Stage 0 of the agent-swarms proposal (#676). With many agents running scripts at once, leftover processes add up quickly.

## Technical description

**`scilink/executors.py`**
- **Own session.** `ScriptExecutor.execute_script` and the warm executor's worker now start with `start_new_session=True` on POSIX (`_NEW_SESSION`).
- **`_kill_process_tree(proc, grace)`.** It sends SIGTERM to the process group, waits `grace`, then always sends SIGKILL to the group, because the leader may exit on SIGTERM while its children ignore it.
  - The group is signalled only when the process leads its own group (`getpgid(pid) == pid` and not our own group).
  - Otherwise, and off POSIX, only the process is signalled. So a registered process that shares the caller's group (as in `test_ui_stop`) never takes the caller down.
- **Where it is used:**
  - a script timeout;
  - `kill_subprocesses_for_thread`, the UI and shell Stop, including best-of-N worker threads;
  - the warm worker's timeout and a failed quit in `close()`.
- **Signals a new session no longer receives.** A script in its own session no longer gets the terminal's SIGINT or SIGHUP, so:
  - an interrupt while waiting on it (`communicate` raising a `BaseException`) kills the tree before re-raising;
  - an `atexit` handler (`_kill_all_registered`) ends any script still registered when the interpreter exits.

**Tests: `tests/test_script_process_groups.py`.** Each case uses a script that starts a 60 s grandchild and records its pid.
- A timeout ends the grandchild.
- A Stop from another thread ends it.
- A `KeyboardInterrupt` during `communicate` ends the tree.
- The exit cleanup ends registered trees.
- A process that shares our group is killed alone.
- A successful script is unaffected.

The first three fail on `main` with the grandchild still alive; the next two fail there because the helpers do not exist. `test_ui_stop.py`, `test_warm_executor.py`, `test_sandbox_env.py`, `test_exec_env.py`, `test_local_executor.py` and `test_scalarizer_sandbox.py` pass unchanged.

**Live verification** (Bedrock `us.anthropic.claude-opus-4-8`):
- An autonomous analysis of a Raman spectrum was asked for a bootstrap in `multiprocessing.Pool(4)`. The generated script started its pool (5 descendants), and the UI's Stop path (`kill_subprocesses_for_thread`) was fired.
- On this branch the whole tree was gone after 0.1 s.
- On `main` it was gone after 1.6 s: Pool workers watch their parent and exit on their own.
- So for a Pool the gain is only promptness. The case this fixes is a child that does not watch its parent, such as an external solver or a plain `subprocess`. The offline tests show such a child surviving on `main`.

- Full offline suite: no failures that `main` does not have.

**Not changed.** Other places run generated or external code through `subprocess.run(timeout=...)` without the executor, so they are not Stop-registered and not group-killed:
- planning's `orchestrator_tools` code runs;
- the scalarizer;
- the LAMMPS, AMBER and packmol wrappers.

Moving them onto the executor is a follow-up.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `974d905d7a63`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
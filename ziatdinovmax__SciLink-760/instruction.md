# Issue

**Title:** Model-written code and external engines run through the executor

## Description

## What this changes

Most code the agents write runs through one executor, which asks for consent, keeps API keys out of the script's environment, applies resource limits, and ends the script (and everything it started) on Stop or a timeout. A few places did not use it:

- **model-written Python** in the knowledge-data query (with no consent check at all), the database screen and the scalarizer: it ran with a `python` found on `PATH`, saw every API key in the environment, and could not be stopped;
- **external engines** (LAMMPS, AMBER tools, packmol, MLIP training, the simulation refinement loop): Stop did not reach them, and a timeout killed only the direct child, so helper processes (an MPI rank, a solver's worker) kept running.

Now every one of them goes through the executor's machinery. Model-written code runs on SciLink's own interpreter with no provider keys, the sandbox limits, and consent asked first. Engines keep their environment (they need licences and `PATH`) but can be stopped, and a timeout ends their whole process tree.

A cancelled run also stops there. When a person presses Stop (web UI or terminal), or a swarm or fan-out item is cancelled (its time budget or a memory guard), the engine it is running is ended and no further engine run starts. Before, the next phase would launch a fresh engine that ran to the end. What the stopped engine wrote is kept beside the run.

Results and error messages are unchanged.

---

## Technical details

- `executors._run_tracked`: `subprocess.run`'s shape for a process SciLink must be able to end (own session / Job, Stop registration, whole-tree kill on timeout or interrupt; raises `TimeoutExpired`). It checks the worker's cancel (`log_context.raise_if_cancelled`) before the process starts and after it ends; with no cancel registered that is a no-op. A stopped run's output travels on the stop, and `LocalExecutor` writes it (and `stopped` as the return code) beside the run.
- `RoutedCapture` and `OutputCapture` register their stop event as the agent thread's cancel for the turn (`log_context.register_turn_stop`, restored on exit), so the user's Stop reaches every wait that polls the cancel.
- `executors.run_generated_script(script, *, timeout, args, cwd, extra_env)`: `sys.executable`, `sandbox_env()`, `_sandbox_preexec()`; `cwd` defaults to the caller's, so relative paths a script was given still resolve.
- `executors.run_engine(cmd, ...)`: a drop-in for `subprocess.run` (`capture_output`, `check`, `stdin`, `stdout`, `stderr`, `text`, `input`, `encoding`, `errors`), parent environment kept. An unknown keyword is a `TypeError`, as in `subprocess.run`, never silently dropped.
- Sites: `query_knowledge_data` (single file and directory databases; consent added, asked once the data is found and before the file / directory paths split, so "no queryable files" and "declined" read differently), `screen_database`, `ScalarizerAgent._execute_script`; `refinement.LocalExecutor`, `mlip_tools` training, `packmol_agent`, `reference_measurement`, `force_field_agent` (4), `amber` (4), the LAMMPS skill's probe.
- Tests (`tests/test_run_tracked.py`): interpreter, no planted `*_API_KEY`, argv; a timeout and a Stop end the grandchild; `run_engine` reads as `subprocess.run` (`check`, `stdin`, capture); a real engine site (`LocalExecutor`) takes its grandchild with it on a timeout; the scalarizer's script sees no keys; `query_knowledge_data` refuses without consent and runs nothing; a cancelled worker running `LocalExecutor` ends at the cancel and never starts its second run; so does a turn stopped through a real `RoutedCapture` / `OutputCapture`, with the first run's output kept; a capture restores the cancel it found; `run_engine` passes `encoding` and refuses an unknown keyword; an uncancelled run is unchanged. A source check, labelled as a lint, pins that no named site calls `subprocess.run` any more.
- Not changed: `git` in the repository loader, the user's own CLI commands, the diagram renderer, the child-process runner and the live loop's own process management.

**Live check (Bedrock).** A scalarizer run on a small table: the model wrote the script, it ran through `run_generated_script`, and the metrics came back as before.

.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `55b86f181ec8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Close a stage's LAMMPS session when the stage raises

## Description

Each stage method (`run_averaging`, `run_integration`, `reversible_scaling`, `temperature_scaling`, `pressure_scaling`, `scan_temperature_range`) creates its own LAMMPS session and closes it as its last statement. So if anything in the stage raises, the session stays open. In library mode that keeps the pylammpsmpi/executorlib workers alive until the process exits.

## Changes

- `BaseRunner` gets `__enter__`/`__exit__`, which close the session on the way out, with or without an exception (`calphy/runner.py`).
- `Phase` gets the same: `__exit__` closes the session opened by the stage it wraps. Stages now create their session with `lmp = self._open_lammps()` instead of `ph.create_object(...)`, so the job remembers it. Stage bodies are otherwise unchanged.
- In `routines.py` (and the averaging call in `dcci.py`), each stage call becomes
  ```python
  with job:
      job.run_averaging()
  ```

On success the stage has already closed the session itself, and the second close does nothing on both backends: `LibraryRunner` checks `_closed`, the executable runner's buffer is already empty, and the restart-file removal checks that the file exists.

`__exit__` never suppresses the exception. If `close()` itself fails while an exception is already propagating, the close error is logged and the original exception is re-raised. A dead LAMMPS worker can easily make the close fail, but the error that matters is the one that got us there.

Setup failures are covered too. `create_object` emits the init commands after it has constructed the runner, and `LibraryRunner.__init__` activates ML-IAP after the executor already exists. If either step raises, the runner closes itself (through `__exit__`) before the error propagates, because at that point nobody else holds a reference to it.

The DCCI driver's own two-cell sessions (`dcci.py`, `lmps[phase]`) are not touched here.

## Testing

- Full suite: 444 passed, 26 skipped. This includes the golden command-stream tests, and the emitted LAMMPS command streams are unchanged.
- New tests: the runner context manager closes on success and on exception, a failing `close()` raises when no exception is in flight and does not mask one that is, `with job:` closes the session of a failed stage, and a failure in ML-IAP activation or in the init commands closes the executor. Both of those fail without the fix.
- No end-to-end run against a real LAMMPS binary.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c07ac2a6a847`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
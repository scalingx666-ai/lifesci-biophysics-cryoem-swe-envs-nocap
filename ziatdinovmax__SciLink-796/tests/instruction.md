# Issue

**Title:** QC: run every deck of a multi-deck reply, keep each deck's log

## Description

## Summary

A molecular QC reply with more than one deck ran the engine with **no input file**. The NWChem skill asks for one deck per phase, but `MolecularQCAgent` set `entry_file` only when a single file came back, and its reply format had no way to say which deck runs. The run command's `{script}` was then filled with an empty string:

```
reply:    hcn.nw, hnc.nw, ts.nw   (no entry_file)
command:  "<nwchem launcher> "    → "failed to open the input file"
```

In a benchmark run, all four cases whose reply had more than one deck failed this way, and every single-deck case ran. The code is the same on `main` (`molecular_qc_agent.py` entry-file rule, `simulation_pipeline._collect_phases`).

A second problem appears once the decks do run. Sequential phases share one run directory, and every run rewrites `run_stdout.log`, so only the last deck's output survived. For NWChem, stdout is the whole output.

## Changes

- **`molecular_qc_agent.py`**
  - The reply carries `run_order`, the deck filenames in execution order (a schema field, plus one sentence in the prompt).
  - New `resolve_run_order()`: one deck sets `entry_file`. Several decks become `phases`, one per deck in run order, each with its own deck plus any unlisted support files.
  - A reply that doesn't say what to run gets **one follow-up** that shows the decks it returned and asks only for `run_order`. The decks are kept, and the attempt is recorded in `run_order_repair`. If the answer is still unusable, generation fails with both reasons.
  - Files left unrun, and an `entry_file` overridden by `run_order`, are reported as warnings on the run.
- **`simulation_pipeline.py`**
  - `_fill_script()` refuses a `{script}` template that has no entry file. It's used for phases, stages and the parameter-convergence ladder.
  - The run is reported as `refinement_failed` instead of being launched.
  - Convergence on a multi-deck run is recorded as `skipped` with a warning, not as an error.
- **`refinement.py`**
  - Every run directory used by more than one sequential phase keeps each phase's log as `phase_logs/NN_<phase>.run_stdout.log`. This covers multi-phase stages and staged steps. The subfolder keeps the copies out of reach of the engine readers, and the names sort in run order.
  - `phase_logs/` left by an earlier attempt is removed when the campaign starts.
  - `single_entry()`: one single-file entry rule, used by phases, staged steps, convergence and the QC agent.
- **Executors**
  - `LocalExecutor` and `ClusterExecutor` clear the previous run's stdout, stderr and returncode before a run. A run that times out, fails to launch or never starts can no longer report the previous run's output, or its rc of 0, as its own.
  - The cluster side uses the new `HPCConnection.remove()`.

## Testing

- **`tests/test_qc_run_order.py` (new, 36 tests)**
  - `resolve_run_order` edge cases.
  - The empty-entry guard, and the pipeline reporting instead of launching.
  - The agent with canned replies: phases built, one repair round, a usable reply costs one call.
  - Per-phase logs kept in order, no copy for a phase that produced nothing, stale copies cleared.
  - `LocalExecutor` launch-failure and timeout paths.
- **`tests/test_cluster_executor.py`:** a job that writes nothing no longer reports the previous phase's rc. This test fails on `main`.
- **Regression:** 22 related test files give identical results to `main` (refinement loop, dry-run gate, health gate, convergence, MLIP loop, QC and ORCA, router, cluster). `test_molecular_qc_output` has the same single failure on `main`.

## Not covered (existing behaviour, not regressions)

- `run_engine` drops a timed-out process's partial output, so a timed-out deck's log is empty.
- ORCA: a run template that hard-codes the output name (`orca {script} > orca.out`) sends every deck to one file.
- The critic sees only the current deck's output. A summary over all decks for `snapshot_run` is a natural follow-up.
- The geometry-consistency check reads the first deck it can parse, not every deck.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `972d8cfa503c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Headless: --data and --metadata reach the -p task

## Description

## Summary

`scilink analyze -p "..." --data spectrum.txt --metadata spectrum.json` ignored `--data` and `--metadata`. The one-shot (headless) run never told the agent which file to analyze. Depending on the run, the agent either asked for a data path nobody was there to give, or went looking in the working directory and analyzed whatever it found. Now both flags reach the task, as they do in the interactive shell.

## Technical description

**The defect.**
- In the interactive shell, `AnalyzeAdapter.initial_turns` turns `--data` / `--metadata` into the session's first turns ("Examine the data at …", "Load the metadata from …").
- Headless mode (`cli/shell/headless.run`) runs a single `adapter.headless_run(agent, task)`. That is `run_task(task, autonomy=AUTONOMOUS)`, and it never reads those turns.
- The parser checked that both paths exist and then dropped them.

**The change.**
- **`ModeAdapter.headless_context(args)`:** a new hook, `None` by default. It names the inputs a `-p` task must also be given.
- **`AnalyzeAdapter.headless_context`:** returns the absolute `data_path` and `metadata_path`, plus `metadata_format` ("JSON: load it" or "text description: convert it to metadata", by suffix, the same rule as `initial_turns`).
- **`headless.run`:** passes the hook's value to `headless_run(agent, task, context=...)`.
- **Every adapter's `headless_run`:** accepts `context` and forwards it to `run_task(task, context=..., autonomy=...)`. All three orchestrators already fold a `context` dict into the task prompt; it is the same contract the meta delegates through. The meta's headless path, which calls `chat`, ignores it: the meta has no data flags.
- **Other modes:** plan mode's `--data-dir`, `--knowledge-dir` and `--code-dir` are constructor arguments, so they reach the agent, and #686 keeps them in the prompt across the autonomy switch. Simulate has no data flags. The meta's `--message` is simply replaced by `-p`.

**Tests: `tests/test_headless_inputs.py`.**
- `headless.run` with the real analyze adapter, the orchestrator swapped for a recorder, receives both absolute paths and the format label.
- A text description is labelled for conversion.
- No inputs means no context.
- Every mode's `headless_run` accepts `context`.

3 of the 4 fail on `main`; the no-inputs case passes on both. The fake adapters in `tests/shell_fakes.py` and `tests/test_shell_headless.py` gain the parameter.

**Live verification** (Bedrock `us.anthropic.claude-opus-4-8`, the real `scilink` entry point): `scilink analyze -p "Identify the phase and fit the main peaks. Keep it quick." --data sample_A7.txt --metadata sample_A7.json --mode autonomous --yes --profile quick --output-format json`, where the file name is not mentioned in the task.
- **This branch:** `success`, 1 analysis in 211 s. It identified the phase ("TiO₂ batch A7 is anatase", from the ~144 cm⁻¹ Eg band).
- **`main`:** `success` with **0 analyses** in 30 s. With no path, the agent searched the working directory, found unrelated pytest logs there, and reported that they "are not scientific measurement data". In an earlier run from an empty directory, `main` instead asked for the data path, which no headless run can answer.
- Full offline suite: no failures that `main` does not have.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f29e2437150c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
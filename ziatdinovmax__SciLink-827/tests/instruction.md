# Issue

**Title:** scilink analyze: targets, time budget and recipe reuse as flags

## Description

## Summary

From the command line you could set the depth of an analysis (`--profile`), but three other decisions that belong to whoever asks for the result could only be written into the task text, where the orchestrating model might or might not act on them:

- which quantities you actually need (`targets`),
- how long an analysis may take (`time budget`),
- whether a new measurement should **reuse the fitting recipe of an earlier run**, so the new row of results has the same columns.

They are now flags on `scilink analyze`:

```bash
scilink analyze -p "Analyze this spectrum" --data s1.csv --targets "G band position" "D/G ratio" --time-budget 300
scilink analyze -p "Analyze this spectrum" --data s2.csv --prior-analysis <first run's output folder> --reuse-locked-script
```

In the live test the task text said nothing about reuse; the second spectrum was fitted by replaying the first run's locked script and produced exactly the same feature columns.

Mission Control (`scilink`) takes `--profile`, `--targets` and `--time-budget` as the default depth for the analyses it delegates.

Things to know: the replay is applied to the first analysis of each dataset (a follow-up on the same data, such as a re-fit after a replay that did not fit, is free to re-derive); the prior run must be of the same kind of data; and, like `--profile`, the flags are not stored in a session, so pass them again when resuming.

## Technical description

### Design

`run_analysis` already applied `orch.default_profile / default_targets / default_time_budget_s` deterministically ("the caller knows what the result is for"); only `default_profile` had a constructor argument and a flag. This extends the same mechanism.

- **`AnalysisOrchestratorAgent.__init__`**: new kwargs `analysis_targets`, `analysis_time_budget_s`, `prior_analysis_paths`, `reuse_locked_script` → `default_targets`, `default_time_budget_s`, and the new `default_prior_analysis_paths`, `default_reuse_locked_script`. `reuse_locked_script` without paths and a non-positive budget raise `ValueError`.
- **`run_analysis`** (`analysis_orchestrator_tools.py`), one block before the kwargs are forwarded:
  - offered only to an agent whose `analyze()` takes the parameters (or `**kwargs`); a custom agent without them runs as before, with a printed note;
  - **forced replay on the first analysis of a dataset** (no earlier agent record for this `data_path` in the session, preparation and derivation records aside) when the session asked for reuse: caller's paths first, `reuse_locked_script=True`, printed like the caller-fixed depth;
  - otherwise the call keeps its own arguments and the caller's paths are appended after its own as reference.
- **Meta**: `MetaOrchestratorAgent(analysis_depth=...)`, validated by the existing `fanout.analysis_depth`, stored as `default_analysis_depth`; `workers.build_child` (the one construction site for the persistent specialist and fan-out workers) builds analysis children with it. A delegation that names a typed depth still replaces it for that delegation: the rule `run_task` already had (`tests/test_depth_channel.py`), unchanged.
- **`cli/shell/modes.py`**: `_scope_flags` (`--targets`, `--time-budget`) on analyze and meta; `--prior-analysis`, `--reuse-locked-script` on analyze; `--profile` on meta. Validation in the parser: empty target names, non-positive / non-finite budgets, missing prior paths, reuse without a prior. Prior paths are made absolute against the cwd. `/status` shows the depth and the prior runs.

### Why the first-analysis rule

The first version forced the replay on every `run_analysis` call. The independent review showed that left no way out: the system prompt tells the orchestrator, after a replay whose verdict is not `good`, to re-run without `reuse_locked_script`, and that re-run was replayed again; an independent verification became a replay too. It also showed that putting the caller's paths first when they are only reference material made a call that replays or edits its own prior run act on the caller's script, since a replay and `script_edits` take the first recipe given.

### Verification

- `tests/test_cli_analysis_scope_flags.py` (new): flags → constructor kwargs for both adapters; parser refusals; constructor defaults and refusals; the per-task depth rule still holding over a session default; and, through the real `run_analysis` tool with a recording agent, the scope and reuse reaching `analyze()`, the path ordering in both cases, the way out of a forced replay, another dataset's first analysis still replayed, a strict-signature custom agent; meta children (persistent and worker) built with the default depth.
- **Live, Bedrock (`bedrock/us.anthropic.claude-opus-4-8`)**, `scilink ... -p`, task text "Analyze this Raman spectrum of a carbon film." with nothing about depth or reuse:
  - `--targets ... --time-budget 900`: the run's `time_budget` record shows `seconds: 900`; verified;
  - second spectrum with `--prior-analysis <run 1> --reuse-locked-script`: `reuse_validity {reused: true, verdict: good}`, "Reused script fits well (R² = 0.9969)", and `features.csv` has the same 31 columns as run 1;
  - meta `--profile quick --targets ...`: the delegated curve fit ran under the QUICK profile.
  - Targets are consumed only inside the verifier prompt and are not written to the run's outputs, so their arrival is shown by the offline test of the real tool, not observed separately in the live run. The live runs were made before the review round's first-analysis rule; that rule is covered offline.
- Full offline suite compared against `origin/main` by failing-test ids: 27 failed / 28 errors on both (the same ids; none branch-only), 5027 passed on `main`, 5051 here.
- An independent reviewer agent read the diff before this PR; its findings are addressed in the second commit.

### Not changed / known

- `scilink serve` and the web server construct the orchestrators without the new arguments and behave as before.
- The flags are not written to the checkpoint (as `--profile` is not). A fan-out branch resumed later runs under the depth recorded on its ledger entry, which is the branch's own.
- A datacube analysis asked to replay a curve run fails with "No approved prior script to replay"; without `--reuse-locked-script` it ignores the prior run, as before.
- `--targets` applies to curve and image analyses; the hyperspectral agent takes none.

### After the review (round 1)

The reuse decision is one function, `caller_reuse()`:

- the follow-up on a dataset is recognised by its canonical key, not the path string, so another spelling of the path (or an omitted one) is not forced to replay again;
- a run that failed outright does not use up the replay, whether it returned an error or raised; a replay that executed and did not fit does;
- the caller's runs are appended as reference only to a call that is not itself a replay or an edit, so they can no longer act as a silent fallback recipe;
- a call that replays a run made in this session keeps it even on a first look, since that run exists because the caller's recipe did not fit.

Re-checked live on Bedrock on the round-1 commit: the flag-only replay of run 1 on the second spectrum, `reuse_validity good`, the same 31 columns. This supersedes the note above that the live runs predate the first-analysis rule.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a4160c400f2b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
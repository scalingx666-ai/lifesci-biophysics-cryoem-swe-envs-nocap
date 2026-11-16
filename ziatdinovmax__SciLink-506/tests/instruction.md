# Issue

**Title:** fix: robust staged MD generation + LAMMPS transport/staging skill guidance

## Description

# fix: staged MD generation validates the split stages (catch a bad split before it runs)

## Problem

Staged MD campaigns (`staged=True`: minimize → NPT-equilibrate → NVT-produce) intermittently failed generation with `[…] full -> refinement_failed`, on two recurring LAMMPS deck errors the refinement loop could not repair at run time:

1. `Unresolved template variables: ['${visc}']` — the model computed shear viscosity *in* the LAMMPS input (an undefined `variable visc`).
2. `fix nvt and fix npt both on group(s): {'all'}` — a produce/equilibrate stage kept both integrators.

Surfaced running an experiment-validated electrolyte use case end to end.

## Root cause (corrected per review — thanks @ziatdinovmax)

`generate_staged_simulation` generates a monolithic deck *only as the source for the model to split*, then splits it into per-phase stages and returns those. Before this PR **the split stages were not validated** — a bad split (a stage that kept both integrators, or an unresolved `${visc}`) went straight to the engine, where the refinement loop's dry-run gate + fixer could not repair the categorical conflict → `refinement_failed`.

The monolithic deck's own validation does **not** gate anything: `_generate_classical_md_inputs` setdefaults `status="success"` and the pre-run `InputValidator` sees only stage 0. (An earlier version of this description wrongly said it did — corrected.)

## Fix

**Engine-neutral, in the MD agent.** `generate_staged_simulation` now:
1. **Validates each produced stage** via the engine-neutral `_validate` and, on any error, **re-splits with the concrete errors as feedback** (one integrator per stage) — up to three attempts. This per-stage structural check + retry is what the PR adds; it did not exist before.
2. **Last-resort per-stage repair:** if a stage is still invalid, runs the same engine-neutral `_attempt_fix` used on the monolithic deck and re-validates, before failing — preserving the recovery path the refinement loop's dry-run gate would otherwise have taken.
3. Only if a stage is *still* invalid does generation hard-fail (`status="error"`), rather than shipping a broken stage to the engine.
4. Unlinks the prior attempt's stage files on each retry (a retry may rename stages, e.g. `minimize` → `equilibration`).

**Skill (`lammps.md`):** a "Transport coefficients" entry stating the deck emits the raw stress log for the `viscosity_greenkubo` analysis skill (never an in-deck `${visc}`), and a positive `unfix`-between-stages template. The `fix ave/time` snippet now defines `variable pxy equal pxy` (etc.), since `fix ave/time` averages variables/computes, not bare thermo keywords.

## Behavior after fix

| Scenario | Before | After |
|---|---|---|
| Clean split | shipped unvalidated | validated, runs |
| Split keeps two integrators / `${visc}` | reached engine → `refinement_failed` | re-split with feedback, then `_attempt_fix`; runs if recoverable |
| Unrecoverable after retries + fix | reached engine → `refinement_failed` | `status="error"` at generation |

## Testing

`tests/test_md_staged_split.py` (no LLM/engine; real `lammps.validate_script`): clean split; conflicted → self-corrects on re-split; persistent-bad → repaired by `_attempt_fix`; unrepairable → `status="error"`.

**End-to-end:** the electrolyte use case now runs — all four members clear deck validation into the staged run, where before every member hit `full -> refinement_failed`.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4bf029cffee6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** sim: system-aware health bands — derive plausible bounds from the system

## Description

Part of #783. (The relative-stability complement and deeper reference-machinery reuse remain follow-ups, so this is "part of", not "closes".) Stacked on #784 (now merged).

## Summary

The health gate (#782) enforces physical-sanity bands as fixed numbers in skill frontmatter. This makes them **system-aware** while leaving the enforcement engine unchanged, and it is **opt-in (default OFF)**.

## Design: the model owns the centre, the code owns the width

The model returns a single **expected value** `e` for the observable in *this* system, in the observable's declared units; **code** turns it into a loose band `[e/K, e·K]` with `K = health_slack_factor` (default 5). The model never emits a width, so it cannot tighten the gate into an accuracy check. A derived centre that falls **outside** the frontmatter fallback band is dropped (a units-slip guard), keeping the frontmatter band.

## What changed

- `HealthBoundsDeriver` (reads the skill's `validation` section, fail-open) loads the frontmatter `health:` specs to know the observables, their `units`, and the fallback band, and passes the generated **deck** + units to the model.
- `health:` specs gain a real `units` field and a `source` (`frontmatter` | `derived`); violations carry the provenance so the critic can question the **band** (re-derive / loosen), not just the run.
- `RefinementContext.health_bounds` + `_merge_health_bounds` merge derived bands over the frontmatter fallback, per observable, before reader resolution.
- Derivation runs **after input generation on the effective engine** (so an MLIP/ASE run grounds on `ase`, per #784), opt-in via `derive_health_bounds` on the `run_simulation` tool (plus `health_slack_factor`). Default-OFF is byte-identical to #782.

## Tests

Clamp / merge / centre-in-band (units-slip) / provenance / tool-wiring / placement — all green on a pristine checkout; #784's MLIP/ase/router suites stay green under the stack.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `22d964afec08`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
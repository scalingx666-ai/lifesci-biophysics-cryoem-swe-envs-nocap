# Issue

**Title:** Select the potential family before classical FF parameterization

## Description

## Summary

.

`_run_workflow_once` ran `ForceFieldAgent.parameterize` (Step 1.5) on any MD structure with a `components.json` manifest *before* the potential selector from #429 was consulted (it only ran inside `_generate_inputs`, in Step 2). Two consequences:

- A system OpenFF cannot parameterize (e.g. a Pt slab) failed with `failed_force_field` and the selector never ran, even with MACE installed.
- A system OpenFF *could* parameterize had `force_field_files` set by Step 1.5, so the selector short-circuited to `forced: force_field`. The selector never actually chose on the `run_simulation` / `run_complete_workflow` path.

This PR:
- Runs `select_potential_family` once, up front, for any MD task without caller-supplied `force_field_files`, and records it in `result["potential_selection"]`.
- Runs classical parameterization (manifest or PDB branch) only when the selected family is `force_field`.
- Passes the decision into `_generate_inputs` via a new `potential_selection` kwarg so it is not made twice. Direct callers of `_generate_inputs` (e.g. the `generate_dft_inputs` tool) are unchanged: the selector still runs there when no decision is passed.

## Live check

On Deception, against the v2 benchmark's `cond_hard_pt111_water` structure (the case that failed in both benchmark runs), with `AvailableSoftware` reporting `lammps` + `mace`:

```
potential selection: family=mlip source=llm (... Pt phase has no transferable classical force field ...)
MLIP selected; skipping classical force-field parameterization.
Selected: mace-mp-0 (mace)
final_status: success, steps_completed: [input_generation], force_field: null
```

## Tests

- New `tests/test_potential_selection_order.py` (5 tests, no LLM/OpenFF): selector runs before parameterization, `force_field` choice still parameterizes, `mlip` choice skips it without the "no manifest" warning, selector called once per workflow, `_generate_inputs` honours a passed decision. All 5 fail on `main`.
- `tests/test_pipeline_refinement_integration.py`: the MD fixture now fakes the selector (it would otherwise make a real LLM call).
- Pipeline-adjacent modules (`test_md_ff_wiring`, `test_refinement_loop`, `test_observable_requirements_input`, `test_structure_class_from_scale`, ...) pass; `python tests/test_simulation_orchestrator.py` targeted tier is unchanged (13/14, the one failure is the pre-existing `EXPECTED_TOOLS` mismatch on `main`).

🤖 Generated with [Claude Code](https://claude.com/claude-code)

https://claude.ai/code/session_01XYvkT6x9VFTBCyY33DMLPL

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7b8e2d137d8f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** fix: refine_structure no longer assumes POSCAR output format

## Description

# fix: refine_structure no longer assumes POSCAR output format

## Problem

When refining a generated structure, the simulation agent reported that *the user had explicitly requested POSCAR format* and flagged a discrepancy between the generation script writing an **extxyz** file and the "requested" **POSCAR** file — **even when the user never asked for POSCAR**.

## Root cause

`refine_structure` (in `simulation_orchestrator_tools.py`) unconditionally appended `". Save the structure in POSCAR format."` to the refinement request whenever the request didn't already contain the word `"poscar"`:

```python
request = original_request
if "poscar" not in request.lower():
    request = request + ". Save the structure in POSCAR format."
```

This is stale code from when POSCAR was the default output format. Today `generate_structure` writes **engine-neutral extxyz** for crystals (the crystal skill declares `output_format: extxyz`; the engine-native POSCAR is written downstream by the engine step). So on every refinement:

- the request told the validator *"the user wants POSCAR"*, while
- the prior generation script actually wrote **extxyz**,

and the validator dutifully flagged a phantom "requested POSCAR vs wrote extxyz" discrepancy that nobody had asked for.

## Fix

Don't assume any default output format. On refinement:

1. If the user **explicitly named** a format in their request, honor it.
2. Otherwise, **round-trip whatever format the structure was originally generated in** (inferred from its filename), so the refinement request matches the prior script.

New helpers on `SimulationOrchestratorTools`:

- `_requested_format(request)` — the format the user explicitly named (poscar/vasp, xyz, extxyz, pdb, cif, lammps), or `None` (the common case). Never guesses.
- `_existing_structure_format(path)` — the format inferred from the already-generated file's name.

Also corrects a stale comment in `generate_structure` that claimed the orchestrator appends a POSCAR-format instruction — it appends the class-derived format (extxyz for crystals).

## Behavior after fix

| Scenario | Before | After |
|---|---|---|
| Crystal (extxyz) + plain request | forces POSCAR → phantom discrepancy | round-trips **extxyz**, no false flag |
| User explicitly asks for POSCAR | POSCAR | POSCAR (honored) |
| Molecule (`xyz`) / biomolecule (`pdb`) | forces POSCAR | round-trips own format |

## Testing

Added `test_2e_refine_preserves_structure_format` (pure, no LLM) covering both helpers and the exact request-building logic — asserts a crystal refine never injects POSCAR and that an explicit POSCAR request is still honored.

The full targeted suite passes except the pre-existing, environment-specific `Tool error paths` failure (a sandbox `PermissionError` on `/nonexistent`, unrelated to this change — it fails identically on `main`).

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4bf029cffee6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
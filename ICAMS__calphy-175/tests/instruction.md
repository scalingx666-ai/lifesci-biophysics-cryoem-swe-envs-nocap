# Issue

**Title:** bug fix

## Description

This pull request introduces a conditional check for the `use_composition_scaling` key in the `comps` dictionary within the `prepare_inputs_for_phase_diagram` function in `calphy/phase_diagram.py`. If the key is not present, it defaults to `True`.

### Key change:

* **Enhancement to composition scaling logic**:
  - Added a conditional check to ensure the `use_composition_scaling` key is explicitly handled. If the key is missing, the code now defaults `use_composition_scaling` to `True`. This improves robustness by preventing potential `KeyError` exceptions.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `301943d3bd90`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
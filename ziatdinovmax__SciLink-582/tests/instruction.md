# Issue

**Title:** Release 0.0.72

## Description

## Summary

Version bump for 0.0.72. Since 0.0.71:

- **Persistent memory in the web UI** (#570): script bank, review inbox with distillation, curated skills.
- **SAM prefers Apple MPS** with an explicit CPU fallback, and runs its full generator on MPS (#572).
- **Live inset streams best-of-N candidate figures** (#573).
- **Worker-agents panel shows image and hyperspectral agents from the start of a run** (#574).
- **BO results say how a recommendation was selected**, with the acquisition optimum next to a planner-chosen point; `physical_constraints` documented as structural feasibility only (#575).
- **Delegation provenance**: the meta records the dependencies it can prove — cited analysis ids, threaded findings, analyses at a recommended point (#576).
- **Image verification pivots off a stalled prescription** instead of repeating it (#577).
- **Execution-environment probe**: the planner and verifier only prescribe capabilities the script interpreter has; the SAM tool reports structured unavailability (#581).
- The Mission Control mode no longer carries a beta label (#580).

## Technical description

Only `pyproject.toml` changes (`0.0.71` → `0.0.72`). Tagging the merge commit `v0.0.72` runs `release.yml`: React bundle, sdist + wheel, GitHub release with the artifacts attached. PyPI upload stays manual (twine) until trusted publishing is configured.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `43c76ace6098`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
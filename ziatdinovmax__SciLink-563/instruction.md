# Issue

**Title:** Release 0.0.71

## Description

## Summary

Version bump for the 0.0.71 release. Since 0.0.70 the web UI gained two tabs:

- **Skills tab** (#561): upload custom skill markdown for the session and browse the built-in catalog by domain, with a viewer.
- **Telemetry tab** (#562): the tool sequence per agent with arguments, results and inline error messages, worker agent action histories, analysis reports, history download, and the delegation dependency graph as plain SVG (fan-outs collapse into one box, edges routed around boxes, verified by a scenario checker). Also fixes the delegations view never reporting a fan-out's group id.

## Technical description

Only `pyproject.toml` changes (`0.0.70` → `0.0.71`). Tagging the merge commit `v0.0.71` triggers `release.yml`: build the React bundle, build sdist and wheel, attach them to the GitHub release, and publish to PyPI.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `15a3f5735104`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
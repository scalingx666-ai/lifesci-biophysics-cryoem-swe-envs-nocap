# Issue

**Title:** Meta: record the delegation provenance the ledger can prove

## Description

.

## Summary

A follow-up analysis that reuses a prior analysis's locked model, or an analysis of exactly the point a planning delegation recommended, depends on that earlier delegation even when a person measured and uploaded the data in between. The meta now records those dependencies in `context_from` on top of what the LLM declared, so the delegation graph shows the iterating loop instead of disjoint chains and the provenance record is complete.

## Technical description

`_open_delegation` unions the declared indices with `_infer_context_sources(index, task, context)` and records the inferred part as `context_from_inferred`. Three proofs, no guesses: (a) a prior analysis id is cited — ids are stored at close from the child's `analyses` (`analysis_ids`) plus `analysis_*` directory names in `files_produced`; (b) one of a prior delegation's findings (≥ 40 chars) appears verbatim in the task/context; (c) every parameter of a point a planning delegation recommended appears as a number (0.1 % tolerance) AND at least one parameter is named — recommended points are read at close from the child's `bo_history.json` (last step's batch) into `recommended_parameters`. Bare-number coincidences do not match. (c') A recommendation the planner STATED rather than produced through the BO engine — e.g. "Next temperature to measure: ≈ 27.5 K" when the optimizer declined to fit on two points — is also recorded at close (`recommended_values`: each number in a recommending sentence with the specific words around it; range endpoints and generic words excluded) and matches a later task that carries the number AND one of those words. The system prompt states the principle in one sentence. `context_from` remains the provenance layer; the graph (PR #562's checker asserts drawn edges == the ledger's relations) reflects it.

Tests (`tests/test_delegation_provenance.py`, 4): close stores ids and the last recommended batch; continuation, loop closure, threaded finding, declared-only and bare-number cases; a single-parameter point needs its name.

### Live check (Bedrock, Opus 4.8, meta session)

Four-delegation loop: #1 series fit (5 K, 50 K) → #2 BO step (the tool declined at two points; the specialist stated 27.5 K) → #3 analysis of the 27.5 K point with the locked model → #4 three-point BO step (33.6 K). The meta declared #2←[1], #3←[1], #4←[3]. Replaying the inference on that recorded ledger (`_infer_context_sources` over the closed entries) yields **#3←[1, 2]** and **#4←[2, 3]**: the loop-closing edges the issue describes, from the record itself. The stated-recommendation rule was added after this run exposed that a heuristic recommendation leaves no `bo_history.json` to match.

### Regression guard

The fusion-independence stamp (`informed_via=fusion_feedback`, which appends the ADDITIVE-ONLY note to a re-analysis task) reads only the LLM-declared `context_from`; inferred edges record provenance for the ledger and the graph and never change the task a specialist receives (test added). Full suite on the merged state compared against main: no new failures — the 22 differing entries are order-dependent live-test skips that pass or skip identically in isolation on both.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cbf8e2980900`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
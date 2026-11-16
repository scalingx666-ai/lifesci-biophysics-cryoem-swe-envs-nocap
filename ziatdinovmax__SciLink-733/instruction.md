# Issue

**Title:** A claim resting on outputs no gate checked stays provisional; a failed derivation is not a result (#722 A, C)

## Description

## Summary

Part of #722 (parts A and C; part B, gating the scalars themselves, follows with #723).

**What went wrong.** A hyperspectral analysis checks the *maps* it produces: on fresh code a reviewer looks at each map, and on a replay a deterministic gate does. A task can also report single numbers, such as a band's position, depth or width, through a separate channel that no check looks at. Even so, when a swarm item posted the run's scientific claims to the shared board, they were marked **verified**, because the run as a whole had passed.

In the live run behind #722, the numbers came from failed fits: positions stuck at the edge of the fit window and depths of about 10⁻¹². The series' conclusion read that as "no change in the bands". It was posted as verified, triggered a planning item, and was counted as independent support when results were fused.

**What changes.**
- **Unchecked numbers are marked where they're produced.** A number reported through that channel is now marked as unchecked by the code that produces it. The mark travels with the result: on each series unit's verdict, on the run's verdict, and on the summary row the swarm reads (`ungated_outputs`).
- **Such claims stay provisional.** On the board, a claim from a run that reported unchecked numbers is **provisional**, and the record names them: "its gate approved the outputs it checks, but the run also reported outputs no gate checked (…), which a claim may rest on". Subscriptions that act only on verified findings no longer fire on it. The recipe (the script that ran and passed) stays verified.
- **A failed derivation's leftovers are labelled.** When `derive_from_outputs` fails its own checks, files it left behind are listed as `unverified_files` with a note not to cite them. More generally, files from any run that failed are no longer listed in `files_produced`, and a warning says where they are.

**Is this tuned to the data?** No. The mark depends only on which channel a number came through: maps go through a check, the separate number channel does not. No value, threshold or name is inspected. A run whose numbers came from good fits is downgraded too. That is the coarse side the issue chose: a claim doesn't say which outputs it rests on.

**How often this applies.** Often, by design. On the reviewer's machine, 181 of 273 hyperspectral analysis folders since August have a generated script that uses the number channel. Many of those numbers are diagnostics or summaries of maps that *were* checked (a count of fitted pixels, a frame average of a reviewed map, an SNR). So expect roughly two-thirds of hyperspectral runs to post their claims as provisional, and subscriptions that wait for verified claims not to fire on them. **That is the intended trade-off, not a regression.** Part B (#722 B with #723), the next piece of work, earns these back by checking the numbers.

**Follow-ups:**
- Part B (deterministic checks on the numbers themselves) will clear the mark for each number those checks pass.
- A structured "which outputs does this claim cite" field on synthesis claims would make the rule per-claim instead of per-run.

## Technical

- **Producer:** `_extract_scalar_records` (`hyperspectral_controllers.py`) adds `"gated": False` to each scalars-channel record. Committed maps carry no flag; they passed the per-map QC, or `map_health` on a replay. `feature_records` (what a later replay reads as its reference) keeps the key.
- **Reader:** `_verification_record.ungated_outputs(features)` returns the names marked `gated is False`. `ungated_outputs_of(full)` returns the stamp's `ungated`; for results from before the stamp, it reads the records.
- **Stamps:**
  - the hyperspectral series unit stamp (`build_series_row`) gets `uv["ungated"]`;
  - `final_verdict_record` adds `ungated`, the union over a single run's `extracted_features` and the successful units' stamps and records.

  `analysis_verdict`'s four keys are unchanged, so the parity test is untouched.
- **Row:** `run_task`'s `analyses` row gets `ungated_outputs` (`[]` when none).
- **Board:** `_claim_verified` downgrades a verified row with `ungated_outputs`, naming up to six. `_analysis_verified` (recipes) is unchanged.
- **C:**
  - `derive_from_outputs` on failure returns `unverified_files` (the files under `results/derive_<id>/`, minus `_scratch/`) and a `note`;
  - `run_task` skips the files of runs whose status is neither `success` nor `partial`, and the "did not complete" warning names their folder.
- **CLAUDE.md:** the swarm board rule gains the ungated clause.

**Tests** (`tests/test_ungated_outputs.py`, real path):
- **The controller marks the scalars.** The real `RunDynamicAnalysisController` runs a script returning a required map and two scalars, the live shape: `435.0003` nm and a depth of `1e-12`. The scalar records come back marked; the map does not.
- **A claim on ungated scalars is provisional; its recipe is verified.** That controller's committed records feed the real `HyperspectralAnalysisAgent` (its pipeline seam returns them), run through `AnalysisOrchestratorAgent.run_task` → `run_analysis` → `board.records_for`:
  - the row is verified, and `ungated_outputs` names both scalars;
  - the claim is provisional, with the reason;
  - the recipe is verified.
- **A run with nothing ungated posts as before:** a maps-only script gives `[]` and a verified claim.
- **A series carries its units' ungated outputs:** a three-cube series has every unit's stamp, and the series stamp, naming the union.
- **A failed derivation is not a result:** a derivation whose product check fails lists the file it wrote as unverified, and `run_task` keeps it out of `files_produced` with the warning.

All five fail with the fix stashed. Neighbouring suites (board, verdict parity, the series and image verdict paths, hyperspectral series and locked replay, derivation, reuse gate, `run_task` status): 199 passed.

Full suite against a `main` worktree: 3995 passed (+5), 24 failed / 29 errors: the same ids as `main`'s baseline, nothing branch-only.

**Live (Bedrock `us.anthropic.claude-opus-4-8`):** `AnalysisOrchestratorAgent.run_task` on a synthetic 20×20×160 transmission cube (two absorption dips near 520 and 700 nm with lateral gradients), asked for each band's depth map **and** its field-averaged position, depth and FWHM as single numbers, with the `quick` profile. Then `board.records_for` on the result.
- The model planned exactly the issue's shape. The required output was `Band1_Depth` (a map); the per-band numbers were "single global/per-band scalars … via the scalars channel".
- **Row:** `verified: true` ("every target passed verification", `qc_gate`), with `ungated_outputs: [Band1_Center, Band1_Depth, Band1_FWHM, Band2_Center, Band2_Depth, Band2_FWHM]`.
- **Board:** both claims are `provisional`, with "its gate approved the outputs it checks, but the run also reported outputs no gate checked (Band1_Center, …), which a claim may rest on". The recipe is `verified`.
- **What it costs:** in this run the numbers were right (522.8 nm against a true ~523, 698.7 against ~698), and the claims are provisional anyway. That is the coarse cost the issue accepted; part B is what earns them back.
- **A naming wrinkle** the reader of the reason should know about: the map and the scalar were both named `Band1_Depth`, so the list names a map's namesake. Only the scalar is marked; the map passed its review.
- Run time: 3.5 min.

## After the review (round 1)

- **The meta's reading of a row.** The system prompt said a row's `verified` covers "the numbers". It now says it covers the outputs its gate checks, and that `ungated_outputs` names numbers no gate checked, which it does not cover.
- **Every reason is given.** `_claim_verified` joins the reasons that hold (an uncertified replay interpretation and ungated outputs) instead of returning the first.
- **Subscriptions, tested.** The real-path tests pass the board's claims to `reactions.matches` with a subscription as the meta declares one. It doesn't match the provisional claim and does match a maps-only run's verified claim.
- **Scope note.** The Summary now gives how often the rule applies (above).
- **Suite:** 3996 passed (+1), 24 failed / 29 errors: the same ids as `main`'s baseline, nothing branch-only.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2b5a53f29e34`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
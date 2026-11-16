# Issue

**Title:** Harmonized pipeline replay: run one frozen analysis script across sibling datasets

## Description

When several sibling datasets (e.g. one hyperspectral cube per experimental condition) are analyzed by independent agent runs, each run invents its own segmentation, binning, and continuum choices — so differences in the extracted numbers can reflect the *pipelines*, not the samples. The fusion stage already detects this confound and prescribes "re-run with a single harmonized pipeline", but nothing could execute that prescription. This PR closes the loop.

**What you can do now:** `delegate_to_analyses(branches, harmonize=True)` runs the first branch as a pipeline **donor**, then replays the donor's approved analysis script **byte-identically** on every other dataset. Extracted magnitudes become directly comparable across conditions, and the follow-up fusion is told so. Each replay still goes through full per-dataset QC — in live validation the followers' reviewers independently rejected two outputs the donor's plan had chosen poorly, proving the replay path verifies rather than rubber-stamps.

**Live result** (three same-exposure cubes of one lanthanide-nitrate solution, donor + 2 replays + fusion in 29 min): two conditions agreed to sub-percent in every reliable absorption-dip depth, while the third showed a near-constant multiplicative inflation across all bands — resolved by the fusion's computed reconciliation as an optical/illumination signature, not chemistry. The same conclusion previously required a hand-written external script.

---

<details>
<summary><b>Technical details</b></summary>

**1. Hyperspectral locked-script replay** (`hyperspectral_analysis_agent.py`, `hyperspectral_controllers.py`)
- `analyze(prior_analysis_paths=[...], reuse_locked_script=True)` — mirrors the curve/image locked-reuse surface (#172); `run_analysis` already forwards both params.
- `_load_prior_dynamic_records` collects approved (`task_success`) scripts from the prior run's `dynamic_analysis_records.json` (result dir, nested dirs, or direct file).
- `SelectRefinementTargetController` short-circuits: one `custom_code` target per approved record, carrying `supplied_script` + the donor's `required_outputs` — zero planning LLM calls.
- `DecompositionController` is bypassed at the composite top (its internal skip-decision LLM call would overwrite a seeded flag); the replayed per-pixel scripts consume the raw cube as always.
- Execution rides the existing `ctx.supplied_script` path (built for bank edit-adapt); the bank exemplar offer is suppressed for supplied targets. Retry budget forced to 0: a failing replay salvages or reports honestly, never regenerates a different method. Mechanical execution repairs remain allowed but flip `replay_verbatim` to false in the target record; the response carries `script_reuse{n_replayed, verbatim}`.
- `run_analysis` tool descriptions updated for the hyperspectral consumption of `prior_analysis_paths`/`reuse_locked_script`.

**2. `harmonize=True` on the fan-out** (`fanout.py`, meta wrappers/tool spec)
- Donor-first: branch 1 runs synchronously; its newest result dir with an approved record is located; follower tasks get a mandatory instruction to call `run_analysis` with `prior_analysis_paths=[<donor dir>]` + `reuse_locked_script=true`; ledger entries re-rendered and stamped `harmonized_with` / `harmonized_donor_index`. Loud fallback to plain independent branches when the donor yields nothing (result records `harmonized.status: fallback`).
- **Gate inversion (found live):** the complementarity gate rightly classifies a same-technique condition series as *redundant* ("a per-condition series to be compared, not fused as complementary modalities") and declined at 0.85–0.9 confidence regardless of task framing. In harmonize mode the gate keeps only criterion 1 — datasets it calls `unrelated` are still pruned — and the non-redundancy criterion is waived by the caller's declaration; autonomous confirm accepts within the existing soft cap. The normal path is byte-identical.
- `fuse_delegations`: `harmonized_branches` in the synthesis prompt (method coupling ≠ independence spend — cross-branch agreement still counts as independent observation), the report JSON, and the tool's return payload.

**Tests**
- `tests/test_hs_locked_replay.py` (7): plan short-circuit and decomposition bypass with an exploding model (any LLM call fails the test), verbatim execution with zero generation calls, mechanically-repaired replay flagged not-verbatim, entry-contract refusals, record-loader discovery.
- `tests/test_meta_fanout_robustness.py` sections 14/14b/14c: donor-first ordering, follower replay instruction + ledger stamps + fused `harmonized_branches`; no-donor-script fallback; redundant-verdict inversion with unrelated pruning while the non-harmonize path still declines. Suite 62/62.
- Existing hyperspectral suites unaffected (68 passed).
- Live: donor + 2 followers + fusion on real 704×704×260 cubes — follower scripts byte-identical to the donor's (hash-verified), `replay_verbatim: true`, independent QC rejections on the donor's weak required outputs handled via the salvage path, computed reconciliation success.

**Known follow-ups:** replay inherits the donor's `required_outputs` contract even where the donor itself only passed it marginally (consider downgrading marginal donor-required outputs for followers); curve/image analogs of harmonize mode are cheap since their reuse surface already exists.

</details>

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c653cbdf8b07`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
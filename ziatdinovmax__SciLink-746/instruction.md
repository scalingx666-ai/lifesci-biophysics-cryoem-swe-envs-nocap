# Issue

**Title:** A fitted hyperspectral scalar is checked: at its bound or at zero it is a failed fit, not a measurement (#722 B1)

## Description

## Summary

Part of #722 (B1, the deterministic checks on the scalars themselves; B2 and B3 are in #735).

**What went wrong.** A hyperspectral analysis checks the *maps* it produces, but the single numbers a task also reports (a band's position, depth or width) passed no check. In the #722 run those numbers came from failed fits: positions stuck at a window edge, depths of about 10⁻¹². Since #733, a claim resting on such numbers stays provisional. That's safe but coarse: the good numbers were downgraded along with the bad ones.

**What changes.**
- **A fitted number is checked.** A script now returns a number that comes from a fit as `{"value", "role", "bounds"}`.
- **A failed check means no value.** A number sitting at a bound it was fitted within, or an amplitude at zero, is reported as a **failed fit, not a measurement**:
  - no value in the feature table, so the optimizer never trains on it;
  - named in `failed_outputs` on the analysis row;
  - named as FAILED on the board and in both the single-cube and the series write-ups.
- **A passed check counts as gated.** A number that passes is gated, so a run whose numbers all pass gets its claims back.
- **A plain number stays unchecked,** exactly as before.
- **Nothing is read from a name.** The check uses what the script declared it fitted.

**Where the thresholds come from.** The measurement on #722 covered 1,227 scalars from 156 real hyperspectral runs. Failed fits sat at 10⁻¹³ to 2×10⁻⁵ σ of the field mean's noise; healthy values started at 0.65 σ. The zero-amplitude bar is 10⁻⁴ σ, five times above the largest failure. The bound check reuses the curve agent's `validate_bound_pinning`, with its tolerance and exemptions; a declared `ratio` is exempt, since a fraction may legitimately rest on its bound.

**Real data (Bedrock `us.anthropic.claude-opus-4-8`, `AnalysisOrchestratorAgent.run_task` → board, the original #722 task on the Holaris DyNO3 cubes):**
- **White-light transmission series (3 cubes, 704×704×260):**
  - the model returned its fitted dip positions, depths and widths declared; about 30 passed;
  - on the two replay cubes, `b450_fwhm` and `b919_fwhm` came back railed at their upper bounds and were reported as failed fits; the board's reason names them;
  - an earlier attempt in the same run showed exactly #722's symptoms (depth on its zero floor, position at its window edge, width at its lower bound), and all three were caught.
- **HeCd emission series (photon-starved):** 9 numbers failed, all widths at a bound or heights at zero. The final report says the failed cells "failed their fit-health checks and are honestly withheld — not read as values".
- **Two bugs the real data caught, fixed and pinned by tests:**
  - **A width pinned in σ read as "inside its bounds".** A script declared a Gaussian's σ bounds [0.3, 17] for the FWHM it returned (40.03 = 2.355 × 17). A value outside its own declared bounds is now **unchecked, never passed** (those bounds belong to another parameterisation), and the contract asks for bounds in the value's own units.
  - **A real height read as zero.** A zero test relative to the declared range turned a 0.6 height with bounds [0, ~10⁶] into "zero". The zero rule now uses only the noise.

**Open, for review:**
- **Diagnostic numbers still keep claims provisional.** The runs also report undeclared numbers: each fit's `_err` uncertainty, an RMS, a pixel count. Under #733's rule any unchecked number keeps a claim provisional, so the white-light claims stayed provisional although every fitted number was checked. Whether declared diagnostic roles (`quality`, `count`) should count as numbers a claim rests on is a policy choice.
- **A resolution-limited width is a limit, not a failed fit.** A width at a one-sample lower bound is an upper limit, not really a failed fit. The check words it as the curve agent's pin rule does.

---

## Technical details

- **`hyperspectral_controllers._check_scalar(value, role, bounds, facts)`** returns `passed | failed | None` and a reason:
  - bounds outside which the value sits → `None` (unchecked);
  - `validate_bound_pinning` on the value → `failed`;
  - `role == "amplitude"` and `|value| < AMPLITUDE_ZERO_SIGMA × facts["sigma_mean"]` → `failed`;
  - otherwise `passed` if a check ran, else `None`.
- **`_extract_scalar_records(result, units, facts)`** accepts a plain number or a declared dict. Records carry `gated`, `check`, `role` and `bounds`. A failed record has `scalar: None` and `raw_value`, and its description begins "FAILED FIT". The facts come from `ctx.session["facts"]`, so replays are checked too.
- **The contract** (`build_code_generation_prompt`'s return format, the only template that describes `scalars`): the declared form, the roles, and bounds in the value's own units. Every retry and repair prompt is that base prompt plus feedback; a test captures the retry prompt and finds the contract in it.
- **Verdict plumbing:**
  - `_verification_record.failed_outputs(_of)`, alongside `ungated_outputs`;
  - the unit stamp `failed_checks`, and `final_verdict_record` → `failed_checks`;
  - the `analyses` row `failed_outputs`;
  - `board._claim_verified` names failed numbers apart from unchecked ones;
  - `feature_records` persists `check`, `role` and `raw_value`.
- **Write-ups:**
  - the single-cube synthesis renders a failed number as "Value: NONE — FAILED …" and a passed one with its check;
  - the series synthesis's "WHAT A GATE CHECKED" block lists numbers that passed their check, and failed numbers per unit.
- **CLAUDE.md:** the anchor-codegen paragraph and the swarm board rule.

**Tests** (`tests/test_hs_scalar_checks.py`, 7, on the real `RunDynamicAnalysisController`, `run_task` and the board):
- the rules (pin, zero by noise, ratio exemption, no-bounds unchecked, outside-bounds unchecked, a huge range never makes "zero");
- the controller's records, and that failed numbers are absent from the flattened table;
- a run whose numbers all pass gets verified claims, and a verified-claim subscription fires;
- a failed number is named "FAILED" on the board, not "unchecked";
- the series synthesis names checked, failed and unchecked numbers;
- the contract reaches the retry prompt.

Hyperspectral QC goldens are regenerated; the diff is the contract text only. Full suite against a `main` worktree: no branch-only failures (`main`: 22 failed / 28 errors).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ff620304bd3f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
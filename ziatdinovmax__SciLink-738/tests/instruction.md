# Issue

**Title:** Replay escalation against one reference curve: gated, located, answerable as the same (#725); a pinned approved fit is degenerate, not salvaged

## Description

## Summary

 and #726, two fixes to how a curve fit's replay or verdict is explained.

**#725: a replay checked against a single reference curve.**

When a recipe from a run with **one** curve (a single run, or a series with no regime plan) is replayed, the drift monitor compares the new measurement to that one curve. One curve carries no information about its own variation, so a sharp feature that moves by a single sample reads as a big difference. On a 128-point step trace a one-sample shift reads **33%** against the 25% bar; against two reference curves it reads 4%. These replays were escalated to a judge, which then had two problems:
- **No "yes" answer.** With no named regimes, the judge could only answer "none" or "cannot tell" when the replay matched its reference.
- **No location.** It wasn't told where the difference was, and guessed wrongly. In the issue's run it put the residual on the plateau when 78% of it was at the transition.

Now:
- **Gated like identity.** A state flag against one reference curve is escalated only when nobody is attending, as an identity flag against one reference unit already was. Otherwise it stays a flag on the record.
- **Where it is.** When it is escalated, the evidence says "ONE reference curve" and gives *where* the difference is, read from the data by the drift monitor (no model).
- **The affirmative answer.** With no named regimes the judge may answer `same_as_reference`.

**Live:**
- **Setup:** Bedrock `us.anthropic.claude-opus-4-8`. A curve agent fitted a 128-point step trace (verified, R² 0.9996). A second agent reused that recipe, unattended, on the same step one sample later.
- **The check:** the state check read 33%, so the replay was escalated as a `flag`. The evidence located the difference at 40.2–41.7 (the step).
- **The judge:** it answered "**the same as the reference**, same interpretation: True — The step edge sits one sample later … the only 'missing' region flagged (x≈40.2–41.7) is just the width of a one-sample shift of a sharp transition. Both plateau levels, noise and overall shape match".

**#726: "salvaged" on an approved fit.** The rule that flags a parameter stuck at its bound (#592) also writes a warning on fits the gate *approved*, and the verdict read every such warning as "salvaged". So an approved Raman anchor with one shoulder width at its bound was reported as salvaged, and so was every unit that replayed it. Such a fit is still not verified: a parameter at its bound is not a measured value, and nothing records which parameters a claim rests on. It is now reported as what it is, for example "degenerate fit: peak_2.fwhm = 40 at its upper bound 40", and so is every unit that replayed it. A fit that really was salvaged (not approved) keeps the salvage reason.

**The issue's design question** (should a pin on a parameter the claim doesn't rest on still withhold?): it still withholds. The curve agent records no "primary outputs" for a fit, so there is nothing to decide which parameters a claim rests on. That is the same gap as #722's per-claim evidence; a structured `evidence_outputs` on each claim would answer both.

**Not changed, a decision for review:** the issue's side effect. When a follower's replay pins a parameter, the fit loop routes it through the correction ladder, which relaxes that unit's bounds. That buys a trustworthy value for that unit at the cost of comparability: its secondary features then come from different bounds than the other units', and the row says `replay_verbatim: False`. The alternative (keep the locked bounds and flag the pin) trades the other way. I left the behaviour as it is; which one a series should prefer is a call for review.

**Is anything tuned to particular data?**
- The single-reference rule depends only on how many curves the reference holds.
- The location is the drift monitor's existing model-free locate, the live loop's.
- The degenerate-fit reason depends only on whether the fit was approved and had a pin.

## Technical

- **`live/drift.py`:**
  - `DriftMonitor.n_seeded` (reference curves seeded);
  - `DriftMonitor.locate_frame(x, y)`: `_locate_in` on one judged curve against `_known()`, where it overlaps the monitor's axis. Same kinds and units as `locate()`; the live loop's own paths are unchanged.
- **`curve_fitting_controllers._identity_of`** records `state_reference_curves` from the regime monitor and, when the distance is above `CERTIFY_STATE_BAR`, `state_regions = monitor.locate_frame(new curve)`. `_record_checks` copies both onto `reuse_validity`.
- **`_replay.escalation_trigger`:** a state flag with `state_reference_curves == 1` returns `"flag"` when unattended and `None` when attended. A record without the count escalates as before.
- **`_replay.escalation_evidence`:** `state.reference_curves`, a `note` when it is 1, and `state.where` (kind, x_from, x_to, x_peak, share).
- **The judge's answer:**
  - `REFERENCE_ANSWER = "same_as_reference"`: `escalation_question` offers it only when no regimes are named, and `read_escalation_answer` accepts it only then.
  - `belongs_to_text` renders it in the controller's message, the orchestrator's `reuse_caveat` and the board's escalation record; it is never taken as `regime_choice.suggested`.
  - The `flag` reason now reads "the replay differs from a SINGLE reference curve or unit".
- **`_verification_record._unit_verdict`:** a fit with `pinned_at_bound` (top level, or under `fit_quality`) and `quality_history.approved` returns "degenerate fit: <describe_pinned> (pinned at bound)" before the salvage branch. The stamped and legacy verdicts share the function, and the parity test holds.
- **CLAUDE.md:** the escalation paragraph (one reference, location, the answer) and the board rule (degenerate fit).

**Tests** (real paths):
- **`test_series_verdict_path.py::test_an_approved_anchor_with_a_pinned_parameter_is_degenerate_not_salvaged`.** The real series driver, with an approved anchor carrying the controller's pin record: the series verdict, and the followers' recipe chain, read "degenerate fit: peak_2.fwhm = 40 at its upper bound 40". A salvaged anchor that is also pinned keeps "salvaged". Without the fix: "salvaged best-available result (unit spectrum_0000)".
- **`test_replay_escalation.py::test_a_state_flag_against_one_reference_curve_escalates_only_unattended_and_says_so`.** A real replay (`qc_try_reuse`) against a single-run prior, on the same step one sample later:
  - attended: flagged (`state_reference_curves: 1`, distance over the bar) and not escalated;
  - unattended: escalated as `flag`, with the evidence carrying `reference_curves: 1`, the note, and `where` at the step;
  - the scripted answer `same_as_reference` is recorded and rendered on the message and the board.
- **`test_same_as_reference_is_offered_and_read_only_without_named_regimes`.**

All three fail without the change. The escalation, reuse-gate, regime-choice, identity, series-verdict and board suites pass (85).

Full suite against a `main` worktree: 4007 passed, 24 failed / 29 errors on the baseline ids. The one branch-only failure was the realtime reuse golden, which records `reuse_validity`: it gains `state_reference_curves: 1`, regenerated (that line is the whole diff). The QC goldens otherwise fail only on the baseline's six.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `25f440263dc2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
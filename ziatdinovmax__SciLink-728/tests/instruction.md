# Issue

**Title:** Hologram preparation: the phase trace is unwrapped in 2D and checked against the map

## Description

## Summary

. The hologram-preparation step turns a reconstructed hologram stack into a phase-versus-time trace (left ROI minus right ROI). That trace could carry jumps of exactly one fringe (2π), produced by how the phase was unwrapped rather than by the sample. In a live run this produced two wrong results:
- **A spurious "transient":** a few frames dropped by 2π right after a step.
- **A step that was 6 rad too small:** about −14 rad instead of −20, on every frame after the transition. It also made forward and reverse steps look asymmetric.

The reconstruction itself was fine, and its QC passed; the slips came from the trace derivation, which no check looked at.

**After this PR:**
- **The trace is computed robustly,** and on real data it agrees with the phase map in every workflow.
- **The tool checks itself:** frames that sit a whole fringe off the map, and a trace step that disagrees with the map step, fail the preparation's QC instead of passing silently.

A second commit fixes an older, separate defect the new check exposed. The two "steady state" fields behind the phase map were plain averages of frames. When a run's overall phase (piston) drifts by a few radians within a window, those frames cancel and the map becomes noise. On one control run the map step read −0.13 rad against −1.4 from the trace. The steady averages now remove each frame's piston first, as the module already assumed. Runs with a stable piston are unchanged (≈0.01 rad); on runs with a drifting piston the map and its coherence numbers change.

---
## Technical details

### Commit 1, c18ffc99: the trace and its QC
- **The bug.** `derive.py` built the ROI trace by a per-frame **1D unwrap along x** of the band phasor across the columns a coherence gate kept:
  - through a column where |P| vanishes, the 2π choice flips frame to frame;
  - across a gap the gate left (true change > π), every frame takes the wrong branch.
- **The reported trace** is now `roi_trace_2d_unwrap`: a per-frame `_unwrap2d` (the map's own unwrap) on the largest connected valid region of the 2× binned grid, with pixel means over band rows × ROI columns. The CSV/JSON schema is unchanged.
- **The old column-profile trace** is kept as `band_profile_trace`, diagnostic only (`cross_checks.diagnostic_column_profile_trace`). Its phasors still feed the coherence columns.
- **`trace_consistency_checks`** runs two checks in code:
  - (a) frames whose trace is within `trace_slip_tolerance_rad` of a non-zero multiple of 2π from `template_amplitude × map_step`, with template coherence ≥ `trace_slip_min_template_coherence`;
  - (b) trace step vs map ROI step within max(`step_agreement_rel_tol` × the larger, `step_agreement_abs_floor_rad`), i.e. the skill's own rule, previously prose.

  It reports per pair in `cross_checks.trace_qc` and overall in `trace_qc_passed`. The four tolerances are `TOOL_SPEC` parameters with what-it-does / which-way-to-tune descriptions. The coherence bar defaults to 0.5, because real post-transition frames sit at 0.4–0.96.
- **The skill's recipe** ANDs `trace_qc_passed` into `qc.passed` and writes failed checks to `qc.notes`.

### Commit 2, 19d0f453: piston removal in the steady means
`_steady_mean` replaces plain means for FA/FB, the split-half check and the dense-fringe finder. It's separate so it can be judged on its own.

### Real data
All nine workflows of a live bundle were re-derived from the saved wrapped stacks (about 30 s each):

| run | old step | new step | map step | old QC | new QC |
|---|---|---|---|---|---|
| insertion A | 25.11 | 25.52 | 25.81 | 4 slipped frames | pass |
| retraction C | −14.34 | −20.21 | −20.05 | step disagrees | pass |
| insertion D | 21.68 | 22.19 | 21.45 | 1 slipped frame | pass |
| other insertions / retractions | | within 0.3–1.3 rad of old | | pass | pass |
| three controls | | | | pass | pass |

The largest residual on a clean judged frame is 4.66 rad (mid-transition, coherence 0.57); flagging starts at 5.28 rad. If clean transition frames ever get flagged, `trace_slip_min_template_coherence` is the knob.

### Tests
7 new, all on the real functions:
- both failure modes plus a single mid-sequence flip (the old path fails its QC, the new one matches the map);
- clean data passes;
- the knobs change the verdict;
- the piston-immune steady map.

`test_data_preparation_helpers`, `test_prepare_data_stage` and `test_skill_loader`: 41 passed, 1 failed. The failure is `test_aimsgb_common_pitfalls_is_now_captured`, which also fails on `main`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ece23e6462cb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
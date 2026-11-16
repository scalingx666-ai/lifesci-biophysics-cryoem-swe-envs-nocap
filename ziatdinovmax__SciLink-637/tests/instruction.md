# Issue

**Title:** afm skill + image codegen: color→value legends and robust per-row leveling

## Description

## Summary

Two AFM/image-analysis hardening fixes, both surfaced by live benchmark runs.

**1. Color-coded maps need a legend.** A color-coded figure — an orientation
map, a phase map, an abundance map, a height map — can't be *read* without a
legend telling you which color means which value. The agent had been producing
these with no colorbar. Orientation is a special case: it's *cyclic* (0° and
180° are the same direction), so it needs a **color wheel** rather than a
straight bar.

**2. Leveling can silently misclassify background as a layer.** In a thickness-
statistics run, a bottom band of bare substrate was left elevated by the
leveling step and then counted as a ~3 nm peptoid layer — corrupting the
per-layer coverage statistics that were the whole objective. Worse, every
substrate-flatness QC metric looked healthy, because the misleveled band had
been dropped from the substrate mask.

No analysis math changes; these make the figures legible and stop a leveling
artifact from contaminating layer statistics.

<!-- Technical details -->
### Technical details

**Legends (fix 1):**
- `instruct.py` (`IMAGE_ANALYSIS_SCRIPT_INSTRUCTIONS`): one principle-stated
  sentence — any color-encoded panel must carry a colorbar/legend mapping
  color→value; cyclic quantities (orientation, phase) use a cyclic colorbar or
  color wheel (wraps 0°≡180°). General, applies to every image analysis.
- afm skill: the orientation-map deliverable (planning) and its validation check
  now require a cyclic color-wheel legend (hue→angle).

**Leveling (fix 2), afm skill only:**
- planning: estimate each fast-scan row's baseline from that row's own local
  substrate (lowest mode / low percentile), not a single global-threshold mask.
  A smooth plane fit cannot remove an abrupt baseline step, and a global mask
  drops the stepped band (its substrate sits above the global threshold) so its
  per-row offset gets interpolated to ~0 instead of measured — leaving the band
  elevated and later misread as a terrace.
- validation: check the leveled substrate is flat across the whole field, not
  just on the mask — a full-width background band assigned to a nonzero terrace
  is a leveling failure, judged from the assignment map's geometry (substrate-
  only flatness metrics can't see it, since the band was excluded from the mask).

Not included (flagged for a separate agent-code change): a geometry sanity check
in the verifier/QC engine that flags a nonzero terrace forming a wide frame-edge
band. That is the more robust catch but touches agent code.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `63273242c936`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
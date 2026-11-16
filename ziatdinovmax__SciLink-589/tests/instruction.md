# Issue

**Title:** Guard scout overlay labels against present-but-None metadata

## Description

.

## Summary

When a series run's metadata carried a `title` key with no value, the scout overlay figure failed to render and the run logged "Failed to create overlay plot: unsupported operand type(s) for +: 'NoneType' and 'str'". The run still completed, but the scouts were compared without the overlay figure. The overlay and single-curve renderers now treat an empty title, x-label or y-label the same as a missing one and fall back to the defaults.

## Technical description

- `scilink/utils/curve_preview.py`: `render_curve_overlay` and `render_curve_single` use `system_info.get(key) or default` for title/xlabel/ylabel; the title is wrapped in `str()` before the suffix is appended. `dict.get(key, default)` only substitutes when the key is absent, so a present-but-`None` value reached `None + title_suffix`. The axis labels did not crash (matplotlib accepts `None`) but rendered blank instead of the intended default.
- Test `tests/test_curve_preview_none_metadata.py`: both renderers with all three keys set to `None` produce a PNG whose title/labels are the defaults; populated metadata still wins.

### Live check (Bedrock, Opus)

A six-spectrum series analysed with `system_info={"title": None, "xlabel": None, "ylabel": None, ...}`. Control: the pre-fix module from `main` loaded from git raises `TypeError: unsupported operand type(s) for +: 'NoneType' and 'str'` on the same metadata. On this branch the scout logged "Generated overlay comparison plot" followed by "Scouted 3 of 6 spectra", and the captured overlay is a valid PNG.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b43964b461e4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** The data-facts guidance states the list's scope: the strongest few features, not every one

## Description

## Summary

.

**What went wrong.** The code-generation prompt's data facts list the strongest field-mean features. Their guidance said "if no feature is listed where the requested one should be, test it there and declare not_measurable if it fails", which reads the list as complete. It isn't. The facts list at most four features per direction, found in the field *mean*, so three kinds of real feature never appear:
- a weaker band;
- a feature confined to part of the field;
- a step such as an absorption edge.

The #722 measurement found healthy Raman G, D and diamond positions 12–28 channels from any listed feature, and X-ray edges never listed at all.

**What changes.** The same guidance now states the list's scope: only the strongest few features, so a weaker, local or edge feature can be real and unlisted. Test the requested feature where it should be, and declare it not measurable only if that test fails.

Both paths already failed safe: the repair fires only on a *listed* feature inside the declared window, and the judge reviews every declaration. This removes the nudge toward a wrong null, and the attempt that nudge cost.

---

## Technical details

- **`_data_facts`:** the closing guidance sentence, with the scope stated using `max_peaks`. It is the only place the sentence lives; every retry prompt is the base prompt plus feedback.
- **Tests** (`tests/test_data_facts_scope.py`, 2, on the real `_data_facts` and `_contradicting_feature`), with a five-band cube whose fifth band is real but beyond the four listed:
  - the guidance carries the scope, and the old wording is gone;
  - a declaration whose window covers only the unlisted band is not repaired (it goes to the judge), and one whose window holds a listed band is.
- **Hyperspectral QC goldens:** regenerated; the diff is that sentence only.
- **CLAUDE.md:** the anchor-codegen paragraph.

Full suite against a `main` worktree: no branch-only failures. One knowledge-base concurrency test failed while live runs loaded the machine; it passes in isolation (18/18).

**Merge note:** #722 B1's PR also regenerates the same goldens. Whichever merges second needs `QC_GOLDEN_UPDATE=1 pytest tests/qc_golden/test_hs_golden.py`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `579d95e34b76`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
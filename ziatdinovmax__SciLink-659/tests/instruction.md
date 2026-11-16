# Issue

**Title:** Release 0.0.82

## Description

## Summary

Version bump to **0.0.82** so a release build can be cut. It ships the React web app with the new plan-review controls from #658: the "Auto-corrected before review" callout with a one-click revert, and the reopen gate (keep / adopt / adopt with changes), in plan mode and Mission Control. No PyPI upload is planned for this release yet.

## Technical description (for reviewers)

- `pyproject.toml`: `0.0.81` -> `0.0.82`. Nothing else changes.
- Tag point for #658 (`fd9c4617`): plan approval settles, `blocking` critic tier + in-place repair, `trigger` on the plan rewrite tools, critic reads the protocol, TEA runs without a knowledge base, review-gate `notice` payload + `FeedbackPanel` controls.
- The web bundle (`scilink/server/static/`) is gitignored and built at release time by `scripts/build_webui.sh` in `release.yml`; this release exists to publish a wheel carrying the updated bundle.
- Validated locally, mirroring `release.yml`: `bash scripts/build_webui.sh`, `python -m build`, `twine check dist/*` (both PASSED), wheel contains `scilink/server/static/index.html`, bundle assets contain the new `feedback-notice` / `feedback-followup` styles, `scilink/server/presenter.py` in the wheel carries the plan-reopen labels.
- After merge: tag `v0.0.82` on the merge commit; `release.yml` attaches the wheel + sdist to the GitHub release. Its PyPI job stays skipped (`PYPI_PUBLISH` unset); no manual twine upload for now.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fd9c4617b969`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
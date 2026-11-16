# Issue

**Title:** Update docstrings and warnings with updated convergence analysis

## Description

<!--
Thank you for pull request.
Below are a few things we ask you kindly to self-check before getting a review. Remove checks that are not relevant.
-->

<!--
Please note any issues this fixes using [closing keywords]( https://help.github.com/articles/closing-issues-using-keywords/ ):
-->

<!--
see https://regro.github.io/rever-docs/news.html for details on how to add news entry (you do not need to run the rever command)
-->

This PR (no logic change) updates the docstrings and warning messages left stale by the recently merged #1984. #1984 changed the forward/reverse analysis so a partial result is returned when MBAR fails at a fraction below 100% of the uncorrelated samples — that fraction is recorded as `NaN` (both directions) and the rest are retained; `None` is returned only when MBAR fails on the full sample set. Here we bring the `ProtocolResult.get_forward_and_reverse_energy_analysis` docstrings (AFE, RFE, SepTop) and their None-entry warnings in line with that behaviour.

## LLM / AI generated code disclosure
<!-- Please update this disclosure to reflect if you did or did not use LLMs / AI to generate code -->
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: no

Checklist
* [x] All new code is appropriately documented (user-facing code _must_ have complete docstrings).
* [x]  ``news`` entry added in #1984 
* [x] Ran pre-commit: you can run [pre-commit](https://pre-commit.com) locally or comment on this PR with `pre-commit.ci autofix`.
* [x] Filled in the AI generated code disclosure.

## Developers certificate of origin
- [x] I certify that this contribution is covered by the MIT License [here](https://github.com/OpenFreeEnergy/openfe/blob/main/LICENSE) and the **Developer Certificate of Origin** at <https://developercertificate.org/>.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5b97ac9499ce`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
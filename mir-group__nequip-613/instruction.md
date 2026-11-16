# Issue

**Title:** Add `map_location="cpu"` to `load_from_checkpoint`

## Description

Minor fix, using `map_location="cpu"` with `load_from_checkpoint`, matching the behaviour elsewhere in `nequip`. Checked and this seems to be the only missing case.

Avoids an issue where e.g. on an 8-rank DDP job, all 8 models load on GPU 0 first (using the `cuda: 0` device tag from the saved model), which causes a hang on startup.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `27d9d2182da9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
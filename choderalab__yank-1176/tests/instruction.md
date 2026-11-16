# Issue

**Title:** Trailblaze: turn off harmonic restraints, resume, and use samples to seed repex

## Description

## Description

- Fix #968: Allow resuming from the trailblaze checkpoint. This finishes up #1159 to recover also the last positions and box vectors when trailblaze is resumed after being interrupted. The frames are saved in a `dcd` file right now, although they are accessed with a utility function that is format-agnostic so we can change it later if this gets complicated.
- Use the samples collected during trailblaze to seed HREX replicas.
- Fix #1137: Change the default trailblaze path to start with turning off the harmonic/flatbottom restraint instead of keeping it always on and reweighting at analysis time. We will be able to expose it as an option once #1139 is implemented.
- Do not restrain receptor atoms right before starting trailblazing. The option is in the API, but we should expose this as well with #1139.
- Remove a lot of warnings due to `yaml.load(loader=...)` not supporting a default value for the `loader` argument.

## To do
- [x] Update docs and changelog.

## Status
- [x] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f767c087dfef`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
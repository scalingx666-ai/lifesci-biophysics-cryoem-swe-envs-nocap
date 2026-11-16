# Issue

**Title:** BUG: combine cmaps when merging Universes (Issue #3672)

## Description

Changes made in this Pull Request:
`Merge()` remaps the connection attributes (bonds, angles, dihedrals, impropers) onto the merged Universe and concatenates everything else as a plain array. `cmaps` is a connection attribute too, but it wasn't in that set, so merging a Universe that carries cmaps raised `TypeError: Encountered unexpected topology attribute of type ...TopologyGroup`. This adds `cmaps` to the set so it's combined the same way as the others, with a regression test that fails without the change.

`UreyBradleys` is another `_Connection` missing from that set, so it has the same latent bug. I kept this PR to just `cmaps`, but I'm happy to generalise it (derive the set from the `_Connection` subclasses) or do a follow-up if you'd rather.

## LLM / AI generated code disclosure
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: no

## PR Checklist
 - [x] Issue raised/referenced?
 - [x] Tests updated/added?
 - [ ] Documentation updated/added? (bug fix, no API change)
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`?
 - [x] LLM/AI disclosure was updated.

## Developers Certificate of Origin
I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `32b7808d4d8c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
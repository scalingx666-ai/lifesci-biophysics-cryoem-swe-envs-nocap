# Issue

**Title:** Stage 3 is on main; three nits from the approval of #708

## Description

## Summary

Bookkeeping after stage 3 of the swarms merged (#708): the proposal and CLAUDE.md now say stages 0–3 and the recipe-reuse PR (#707) are on `main` and stage 4 (scheduling) is next, and the three one-line nits from #708's approval are fixed here rather than carried: the count of `rests_on` ids dropped includes the ones over the cap; the gate for an undo of an undo names the finding's own withdrawal reason (not the undo's); an act that only lifts one of two withdrawals of a finding says so instead of reporting an empty effect. The approval's other nits (the effect computed outside the lock, a superseded finding reported as brought back, an already-tainted approved finding) are recorded as open under "After stage 3".

## Technical description

- `swarm.py`: `rests_on_dropped = len(raw) - len(kept_ids)`; the board is asked with `Board.has(fid)` (under its lock) instead of reading `_by_id`.
- `board.py`: the undo gate walks the retraction chain to the finding it concerns and shows the FIRST retraction's reason with the undo depth; when no finding's standing changes, `effect` and the gate name the retraction that takes or loses effect.
- Tests: `test_swarm_reactions.py` +1 (two retractions standing on C; lifting one), and the undo-of-undo and `rests_on` tests extended.
- Verification: swarm/board suites 79; full suite against the `main` baseline: 24 failed / 29 errors, the same ids as `main`; no branch-only failure (3911 passed).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a7663b2b4cac`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
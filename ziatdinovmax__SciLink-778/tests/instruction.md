# Issue

**Title:** Promoting a bank record to staging takes the bank's lock

## Description

When several SciLink sessions or swarm items run at once, they share one script bank under `~/.scilink`. Every update to a bank record already waited for the others, except one: promoting a record to the review queue (staging). The analysis agents promote records on their own, so two parallel items could promote the same record at the same moment. It then appeared in the review queue twice, and a success counted by another session in that moment was lost. Promotion now waits its turn like every other update.

The agents also added a second copy without any race: when a record was already waiting for review, an agent staged the script again on its own, unlinked from the record. They now skip it.

This was the last open race in #398. The others were fixed earlier, mostly while making the store safe for swarms:
- atomic writes everywhere (#512);
- skill graduation and memory edits under one lock per skill (#697);
- the other script-bank updates under the bank's lock (#656);
- knowledge-base builds under a lock per name (#683).

.

---

### Technical details

- **The fix:** `promote_to_staging` is now `_locked(_promote_to_staging_unlocked)`, like `add_record`, `record_success` and the other bank read-modify-writes.
  - It reads the record, checks for a staged copy still under review, calls `_staging.stage_solution` and rewrites the record with `promoted_to_staging`. All of that now runs under the bank's per-domain lock.
- **Lock order:** `stage_solution` takes no lock (it writes a new uuid file), and nothing in `_staging` calls back into the bank, so there's no opposite order and no deadlock risk.
- **The agents' nomination** (curve, image and hyperspectral `_maybe_stage_t2_solutions`) fell back to `_staging.stage_solution` on any result other than `success`. That fallback is meant for a disabled bank, but it also fired when the record was already awaiting review, staging a second copy with no `bank_id`, race or not. The refusal now carries the staged id (`already_staged`), and each agent skips the item on it. The direct fallback stays for a disabled bank and for a record that is gone.
- **Callers:**
  - the curve, image and hyperspectral agents' automatic nominations;
  - `scilink memory`;
  - the web memory panel;
  - `promote_many`, which takes the lock per record, since the lock is reentrant within a thread.
- **Tests** (`tests/test_script_bank_hardening.py::TestConcurrentWriters`):
  - Two threads promote one record while staging is slowed down. One gets `success`, the other "already staged", and one staged copy exists.
  - A `record_success` from another thread during a promotion is kept.
  - The locked-names check covers `promote_to_staging`.
  - All three fail on `main`.
- **Test** (`tests/test_persistent_memory.py`): two nominations of one banked script through the real curve agent, one after the other and from two threads at once, leave one staged copy linked to its record. It fails on `main` and on c9ba6e7f.
- **Verification:**
  - The bank, staging, memory and graduation modules pass (207 tests).
  - Full suite: the same 41 failing tests as `main` (2295d58f), by test id, and 2 more passing.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2295d58f851b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Derive the history-trim trigger from MAX_HISTORY_MESSAGES, not a 120 literal

## Description

## Summary

Each chat orchestrator trims old conversation turns when a session gets long, and `MAX_HISTORY_MESSAGES` is supposed to control that. In reality it only controlled *how much* survives a trim — *whether* a trim happens was decided by a hardcoded `len(messages) > 120` in every chat handler. So configuring a smaller history cap did nothing at all until message 121, then chopped ~100 messages in one step; this bit us live while testing the session-memory feature (#513). The trigger is now derived from the cap plus a named hysteresis, so a configured cap actually moves the trigger, while default behavior stays exactly as before (100 + 20 equals the old literal).

Along the way the new tests exposed a second, nastier edge: with a cap of 10 or less, the trim's arithmetic produced `history[-0:]` — the whole list — so a "trim" *grew* the history with duplicated messages. Also fixed.

---

**Technical details**

- All 8 gates (both chat handlers × four orchestrators) now read `len(self.messages) > (self.MAX_HISTORY_MESSAGES + self.TRIM_HYSTERESIS)`; `TRIM_HYSTERESIS = 20` is a new class constant next to the cap. The hysteresis preserves the intended sawtooth (a session at the cap isn't re-trimmed on every turn), and the default `100 + 20 == 120` keeps default behavior byte-identical.
- The planning orchestrator had no `MAX_HISTORY_MESSAGES` at all — bare `100`s at four call sites and in `_trim_history`'s signature default; it gains the same constants and the call sites now reference them (same values).
- Small-cap fix: in the marker-style `_trim_history` (analysis / planning / meta), `recent_window = max_messages - context_window` hit 0 when the cap ≤ the 10-message head window, and `history[-0:]` returned everything. The head window now scales as `min(10, cap // 2)`; unchanged for any cap ≥ 20 (so the default path is untouched — sim's slice-trim never had the bug).

**Verification**

- Offline (`tests/test_trim_gate.py`, 7 tests): default gate value is exactly 120 on all four classes; no `> 120` literal remains in any handler (source inspection); a lowered cap actually triggers the trim on all four orchestrators against a stubbed LLM; small caps trim without growing or duplicating.
- Regression: dispatch guards + UI-stop + planning campaigns + sim delegation (28 tests) and fan-out robustness 62/62 green; all 8 LLM-facing surfaces (system prompt + tool schemas × four orchestrators) hash byte-identical to main.
- Live (Bedrock opus-4-8), 9/9: analysis with cap 30 trims at the new gate of 50, recalls a head-window fact exactly, and does not fabricate an evicted value; analysis at default config shows no trim at 118 messages and the same trim-to-~100 past 120 as before the fix; sim with cap 30 trims and replies coherently.

Related: found while building #513 (the session event log), whose trim-marker now points trimmed sessions at `search_session_history` — after both merge, an evicted fact is recovered from the event log instead of being reported as unavailable.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `60b33703c275`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Per-tool checkpointing, informed restore, and resumable fan-outs

## Description

Crash-and-resume support across all four chat orchestrators, extensively live-validated on Bedrock (8 kill/restore scenarios, 62 live checks).

**What changes for users:**

1. **A crashed or killed session loses at most one tool call of work.** Previously the session saved only at the end of a turn — and one turn can contain an entire analysis, delegation, or parallel fan-out, so a mid-turn crash lost all of it. Every orchestrator now saves after each completed tool step (quietly, throttled so it costs nothing noticeable).

2. **A restored session knows where it stopped.** On restore after an interruption, the model is told which step completed last and continues from the recorded results instead of redoing work. Live: a session killed right after a curve fit finished was restored and reported the fit (R² ≈ 0.998, both peaks) without re-running anything.

3. **Interrupted parallel analyses are resumable.** If a session dies mid-fan-out, restoring it now shows a notice, and asking to "finish the interrupted analysis" resumes only the unfinished branches — in their original folders, with completed branches untouched — then fusion proceeds as usual. This works even if the resume itself is interrupted, and for harmonized runs (the resumed branch still replays the donor's script verbatim).

4. **A runaway tool result can no longer break a session.** Found during live testing: one tool echoed a 7.7 MB file into the conversation, making every later model call fail. Oversized results are now truncated with a recovery note (the tool's saved files and stored state are unaffected); normal results are untouched byte-for-byte.

---

**Technical details**

Implements `phase3_checkpoint_resume_remap_plan.md` (the remapped Phase 3 of the fan-out plan): resume as shared orchestrator-level infrastructure, with nothing added below the orchestrators (foundation-agent pipelines keep resume-by-re-derivation via the script bank / locked replay).

- **Per-tool checkpointing (`f7f1d655`).** `_checkpoint_after_tool()` after each tool iteration in both chat loops of all four orchestrators (copy-shaped per the no-base-class rule), mirroring the MCP server's `_checkpoint_after_tool`. Quiet (`quiet=`/`verbose=False` on `_auto_checkpoint`; simulate gained `force=` past its N-message gate) and wall-clock-throttled (15 s; an expensive call always lands one). Invariant: persist points follow tool results, so mid-turn saves are pair-complete — no dangling `tool_use` reaches disk from this path.

- **Informed continuation (`385166db`).** `close_interrupted_turn`'s stub (already at all eight turn-start seams) now names the last completed tool call(s) and instructs continuation from recorded results; healthy histories byte-identical, malformed `tool_calls` fall back to the generic note.

- **`resume_fanout` (`62baea87`).** New meta tool + restore-time notice. Reopens ledger entries left `running`/`interrupted` (the turn-start sweep converts the former to the latter; budget-degraded `timed_out` verdicts stay final), rebuilds each branch child in its original `fanout/<NN>_<slug>/` dir with `restore_checkpoint=True`, and re-runs the recorded pre-meshed task verbatim (+ resume note) through `_run_one_branch` — budget/cancellation shared via the extracted `_cancel_overdue_branches`. Three persistence gaps found by the live drivers are fixed here: provisional entries checkpoint at fan-out launch (previously a crash before the first branch completed left no on-disk trace), harmonize-mode follower tasks + `harmonized_with` stamps checkpoint before followers run (previously a crash mid-replay resumed them silently unharmonized), and resume checkpoints its reopened entries so it converges under repeated interruption.

- **Tool-result bounds (`a4096c86`).** `clip_tool_result` (200 k chars, recovery marker) at the message-build chokepoints of all four orchestrators — `build_tool_message` clips the text part only, images unaffected — plus `load_metadata`'s echo capped at 20 k (`_bounded_metadata_echo`; full metadata still stored). Root cause of the one live failure observed during the edge sweep (5.3 M-token prompts, durable via per-tool checkpoints).

**Live validation (Bedrock, all via SIGKILL harnesses):** Phase A kill/restore 8/8; Phase B post-fit kill 6/6; Phase C fan-out crash/resume 10/10; edge sweep — multi-turn mid-kill 8/8, simulate-orchestrator copy 7/7, partial fan-out resumed through the meta LLM's own tool choice 7/7, double kill (resume killed mid-resume) 9/9, interrupted harmonized replay 7/7.

**Offline:** 34 new tests (`test_per_tool_checkpoint`, `test_resume_fanout`, `test_tool_result_clip`, plus interrupted-note cases in `test_stop_turn_alternation`); several assert the on-disk checkpoint state from *inside* a running branch. Full 149-test sweep over the touched suites green.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c5866ae4a430`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
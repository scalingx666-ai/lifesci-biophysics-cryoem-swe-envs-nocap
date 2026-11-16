# Issue

**Title:** Activity line narrates plan refinement instead of showing "-"

## Description

## Summary

In the web UI, the small status line next to the spinner ("what the agent is doing now") showed just "-" during plan refinement in planning mode. It now narrates the actual stage: "Refining Plan based on Results...", "Searching local KB for context on results...", "Reasoning over results...", then "Waiting for your input…" while the feedback panel is up, and "Wrapping up…" at the end of the turn.

Nothing else in the UI changes. The verbose log, the feedback panel, and the analysis-mode status lines behave exactly as before.

## Technical description

### Root cause

`currentActivity` in `webui/src/components/LogView.tsx` scans the narration tail backwards and returns the first line matching a milestone pattern. The CLI narration frames its human-in-the-loop prompts with bare rules (`print("-" * 60)`). The `--- title ---` pattern (`^-{2,}\s*(.+?)\s*-{2,}$`) matches a dashes-only line with a title of a single "-", so the pill rendered "-" from the moment the feedback banner printed. Separately, the refine path's own action lines ("⚡ Tool: Refining Plan based on Results...", "Reasoning over results...") matched no pattern, so the pill fell back to whatever older line was recognizable.

### Change (one function, `currentActivity`)

- Skip lines made only of rule characters (`-`, `=`, `_`, `*`, and the box-drawing variants) before any pattern runs.
- Recognize the HITL banners the planning agent prints (`REQUESTING FEEDBACK`, `SELECT A PLAN CANDIDATE`, `CODE REVIEW REQUIRED`) as "Waiting for your input…", so the pill does not keep showing the last action as in progress while the turn is parked on the feedback panel. `FILES PRODUCED THIS TURN` reads as "Wrapping up…".
- The generic action-line fallback also strips a leading `Tool:` (alongside the existing `LLM Step:`), and its verb list gains the verbs the planning narration uses (Refining, Reasoning, Processing, Saving, Verifying, Updating, Building, Synthesizing, Retrieving, Querying, Inspecting, Reviewing, Drafting, Writing, Analyzing), taken from a tally of the planning agents' print statements.

Shipped bundle in `scilink/server/static/` rebuilt.

### Verification

Replayed the refine-with-results narration (tool announcement, refine header, KB search, reasoning, feedback pause, code-review pause, candidate pick, files-produced banner) through the old and new function in node:

| stage | before | after |
|---|---|---|
| tool announced | null | Refining Plan based on Results... |
| reasoning | (stale KB-search line) | Reasoning over results... |
| feedback / code review / candidate pick | `-` | Waiting for your input… |
| files produced | `-` | Wrapping up… |

Seven unrelated probes (pipeline STEP names, verification counters, 🤖 answer, best-of-N candidate tags, ✅ outcomes, `--- title ---` headers, 💭 thoughts) return byte-identical strings before and after. `tsc -b` clean.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `78704328ded5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Route image trend-codegen through the compile-checked parser

## Description

## Summary

When analyzing an image series, the final trend stage asks the LLM to write a small Python script that plots how the extracted features evolve. That stage was still reading the LLM's reply with an old, fragile parser — the one code-generation site missed when the rest were hardened in #240. On weaker models this is exactly where a garbled reply could slip through and be executed, or fail confusingly. The trend stage now uses the same robust parser as everything else: replies are checked for valid Python before running, recoverable formatting mistakes are salvaged, and cut-off replies are recognized and retried.

.

---

**Technical details**

- `ConditionalImageTrendController._generate_trend_script` and its `_correct_script` now call `parse_codegen_response(response, field="script", logger=self.logger)`, mirroring the curve trend controller converted in #240 byte-for-byte. The constructor keeps its (now unused) `parse_fn` arg, as the curve twin does.
- This adds the `ast.parse` compile-check, JSON-escaping salvage, and distinct truncation detection (`finish_reason=length`), and removes the legacy balanced-brace fallback that could return a wrong embedded dict.
- Offline regression (`tests/test_image_trend_codegen_parse.py`, 5 tests): JSON and fenced responses parse with side fields preserved; a non-compilable script and a truncated response are rejected rather than executed; the correction path returns fixes and rejects garbage.
- Live-validated on Bedrock (`bedrock/us.anthropic.claude-opus-4-8`): a 3-image synthetic particle-coarsening series ran the full pipeline; the trend stage generated and executed `trend_analysis.py` through the new path and produced a correct dashboard — mean diameter 15→35 px over 0→20 min matching the planted ground truth (radii 5/9/14 px), linear coarsening fit 0.99 px/min (R²=1.000) plus an LSW cube-law panel.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1bf34534b9ec`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
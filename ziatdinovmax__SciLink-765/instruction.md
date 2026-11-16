# Issue

**Title:** Deck-grounded analysis: resolve self-named outputs, identify columns from the deck

## Description

## What

Makes the **run deck the ground truth** for analysis inputs, in two places where fixed filename/header patterns were too brittle.

**1. Deck-grounded output resolution (`resolve_outputs`).** The analysis maps a run's output files to data kinds from each engine skill's `outputs:` patterns — but an output written to a self-named file (a Green–Kubo pressure tensor sent to `stress.dat` by `fix ave/time … file stress.dat`) matches no pattern and never reaches the analysis. Now an LLM maps the OUTPUT files to the skills' data-kind vocabulary using a header peek of every file plus the input deck as context. It's a strict superset of the static match: the static map is the floor, the LLM can only ADD existing non-empty files, it is skipped entirely when nothing is unclassified (no wasted call), and it falls back to the static map on any error.

**2. The deck reaches the analysis (`INPUT_DECKS`).** Column identity — which column of a stress file is Pxy/Pxz/Pyz — can't be read reliably from header labels (`v_pxy`, `v_sxy`, `c_p[4]`, `Pxy`); only the deck knows. Engine skills now declare their input-deck filename convention in an `inputs:` frontmatter block (mirroring `outputs:`); `run_analysis` gathers the deck(s), skips dry-run artifacts, and passes their text to `compute_property`, which injects an `INPUT_DECKS` preamble beside `DATA_FILES`. Generated analysis code maps columns by tracing the deck (`v_<name>` → its `variable`, `c_<id>[i]` → its `compute`) instead of guessing.

**3. Absolute paths.** `run_analysis` resolves `run_dir` so `DATA_FILES` entries open from the sandbox's own working directory — a relative `run_dir` (common from a tool call) otherwise yields unopenable paths.

## Why

General, engine-neutral analysis infra — no filename or engine specifics in agent code (engine skills declare them). It's what lets a multi-replica Green–Kubo analysis actually find each replica's stress series and parse its columns; the MD-transport convergence work builds on it.

## Tests

20 analysis-agent tests: resolver (adds a deck-named file, no-LLM-when-static-complete, error fallback, unknown-kind/missing-path hygiene), `inputs:` frontmatter, deck gather + dry-run skip, `INPUT_DECKS` reaching the codegen, and relative-`run_dir` → absolute `DATA_FILES`.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `47a3de5f8a5c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
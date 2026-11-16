# Issue

**Title:** Attach a script in chat: run_analysis(reference_scripts=...) with script-bank semantics

## Description

## Summary

You can attach your own analysis script in chat and tell the agent to use it. Stacked on #555, which it builds on.

- In analyze mode, drop a `.py` on the paperclip. It lands in the session's `scripts/` folder and the draft already says "use it as the reference implementation, adapt it to the data as needed, and tell me what you kept and changed". Send that with your data and the agent takes it from there.
- The agent treats your script the way it treats a proven script from its script bank: it adapts rather than reimplements, keeps your method, model choices, parameters and thresholds unless the data demonstrably needs a change, conforms the input and output to its run, keeps your function names where practical, and states in its answer and in the generated script's header what was kept and what changed. The result goes through the normal sandbox and quality loop, so a mangled adaptation is corrected, not silently wrong.
- It is never run verbatim. That is deliberate: verbatim execution of arbitrary code is what MCP servers are for. No SciLink-specific tool format is involved anywhere.

Verified live on the EELS identification example with the included `examples/eels_identification_demo/user_edge_fit.py`: the generated fitting script is headed "Adapted from user_edge_fit.py" with KEPT and CHANGED sections, reuses the script's functions and constants by name, and the answer to the user lists exactly what was kept (power-law background, Gaussian peaks, area formula, output style) and what changed (onset model, peak counts) and why.

## Technical description

### Design

A user script is reference material with script-bank semantics, but it arrives as a bare file rather than a prior run's artifact bundle, so it gets its own channel rather than overloading `prior_analysis_paths` (whose loaders expect `series_fit_results.json` and whose hyperspectral form refuses non-locked use).

- `scilink/agents/exp_agents/_reference_scripts.py` (new, stdlib only): `load_reference_scripts(paths, max_bytes=64_000)` reads text scripts, skipping missing, binary or non-text files with a warning and truncating long ones with a note; `reference_script_block(state, heading)` renders the block with the adapt-not-reimplement guidance and each script in a fenced code block. Empty when there is nothing, so callers append unconditionally.
- `CurveFittingAgent.analyze`, `ImageAnalysisAgent.analyze`, `HyperspectralAnalysisAgent.analyze` take `reference_scripts: list | None` and put the loaded scripts in pipeline state. The hyperspectral agent threads it through `_run_analysis_pipeline` into both of its state dicts.
- Controllers: wherever a controller shows the user's `analysis_hints` the block follows. Planning, code generation, plan refinement and script correction, 12 sites across the three controller modules. The two correction prompts that splice guidance in front of the response marker splice the block the same way, so a fix cannot drift away from the method the user asked for.
- `run_analysis(reference_scripts=[...])`: forwarded by signature introspection like the other optional knobs, relative paths resolved against the session directory, schema text stating when to pass it. The analysis orchestrator prompt gains one sentence. The meta needs nothing: it passes the path in the delegation task and the analysis child does the rest.
- Web: new `scripts` upload category (`SUPPORTED_CODE_EXTENSIONS` → `<session>/scripts/`); the paperclip routes `.py`, `.yaml`, `.yml`, `.md` there in analyze mode, and a script-only attachment produces the reference-implementation draft text. Plan mode keeps routing code to `code/`.
- `examples/eels_identification_demo/user_edge_fit.py`: a realistic user script (power-law background, noise-threshold onset, Gaussian near-edge peaks, JSON and figure) that runs standalone on the example spectrum.

### Tests

`tests/test_reference_scripts.py` (6): loader edge cases (binary, non-text, missing, truncation); block content and both heading styles; all three `analyze` signatures; every `analysis_hints` site in the three controller modules carries the block (a source-level invariant, so a future guidance site cannot forget it); the schema text; the `scripts` upload category and that the data category still rejects `.py`. Agent suites (`test_curve_fitting_*`, `test_hyperspectral_*`, `test_hs_locked_replay`, `test_spectral_unmixer_mask`, `test_web_server`): pass. Full suite versus `main`: identical failure set (the one order-dependent `test_trim_gate` case excepted), 24 more passing tests.

A first full-suite run caught a real defect: the hyperspectral state dict I had patched lives in a helper that did not receive the new parameter, so two hyperspectral tests failed with a `NameError`. Threaded properly and re-run clean.

### Live verification (Bedrock Opus 4.8, autonomous analyze session)

Prompt: the example spectrum and metadata plus "I attached my script `user_edge_fit.py` — use it as the reference implementation (adapt it to the data as needed) to fit the Ti L2,3 and O K edges, and tell me what you kept and what you changed."

- The plan named the script's method before any code was written ("power-law background A·E^-r over the pre-edge window").
- The generated `fitting_script.py` opens with "Adapted from user_edge_fit.py", a KEPT list and a CHANGED list, and reuses `power_law`, `gaussians`, `analyze_edge`, `EDGES`, `NOISE_SIGMA_THRESHOLD`, the `Ti_L23` / `O_K` keys and the FWHM constant by name.
- The run passed the quality loop at R² 0.999 and the answer reconciled the result with the script: kept the per-edge power-law background, Gaussian peak parameterization and area formula, the two-edge structure and the JSON-plus-figure output; changed the onset detection to a fitted edge step and the peak counts from two per edge to four and three, with the reason for each.
- Cost note: this was a thorough best-of-N run with annealing and took about 25 minutes; the file-tool turns in #555 take seconds.

### Not in scope

Verbatim execution of a user script in analyze mode (a foundation-agent contract change; MCP is the verbatim route today), and per-user script libraries.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cf7de4f7c275`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
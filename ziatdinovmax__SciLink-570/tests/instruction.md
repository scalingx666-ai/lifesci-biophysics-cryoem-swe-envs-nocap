# Issue

**Title:** Skills tab: persistent memory — bank, review inbox, skills, distillation

## Description

## Summary

The web UI's Skills tab now carries the **persistent memory** panel that the Streamlit app had: the knowledge SciLink accumulates across sessions under `~/.scilink`, in the order it flows.

1. **Script bank** — every approved analysis banks its working script. Records that keep succeeding across sessions are marked proven; you can inspect any record (its fields and the script), nominate one for review, nominate a group of same-system variants under one technique label, or delete it.
2. **Review inbox** — nominated scripts, error lessons and your own feedback wait here, grouped by technique. Inspect a record, discard it, or select records and distill them: into a **new** skill, or into an **existing** one where you review the proposed merge as a diff, optionally edit it, and apply with a backup. Distillation is one large model call (1–3 min) and runs in the background while you keep working.
3. **Skills** — provisional skills wait for your approval; approved ones auto-route. View, edit (validated, backed up), approve or suspend, delete, and for a fork of a built-in skill, diff it against the shipped version.

The on/off switch is here too, persisted to the store; the panel says so when the `SCILINK_MEMORY` environment variable overrides it.

## Technical description

- `scilink/server/memory_api.py` (new): plain functions over the existing backend (`scilink.skills._shared._memory` / `_staging` / `_script_bank`, the same one `scilink memory` and the Streamlit panel use). `memory_overview()` returns everything the panel shows in one call (switch + env override, pipeline counts, bank by domain with `find_variant_groups` suggestions, inbox by domain/technique with same-session *related* records from other labels, skills with a `shadows_builtin` flag) and never raises for a hostile or corrupt record. The technique-match heuristic (`_skill_tokens` / `_record_tokens`) is ported from the Streamlit panel to order upgrade targets and flag mismatches before the expensive call. The two LLM operations run as background jobs (`_start_job`, polled via `/memory/jobs/{id}`) using the named session's model (`session.agent.model.generate_content`), because a web request cannot block for minutes the way the Streamlit spinner did; a built-in target is previewed on a temporary copy and forked only on apply. Guards: memory off, fewer than `consolidate_min_n` records, a label that would sweep in unselected records (409 naming them), unknown records / session.
- `app.py`: endpoints under `/api/v1/memory` (see the table in `docs/react_web_ui.md`); `MemoryError(status, message)` → `HTTPException`. One store per server host, shared by every session and every user of a multi-user server.
- `webui/src/components/MemoryPanel.tsx` (new), mounted under the catalog in `SkillsPanel`: the switch, pipeline strip, three stages with lazy record inspectors, the distill flow (selection incl. related records, new/existing destination, swept-label guard computed client-side, target select with ✓/⚠ badges, job progress, diff review with optional editing and a debounced re-check of additivity warnings, apply/cancel), and skill rows. Destructive actions are two-click confirms — no browser dialogs. `api.ts`: types + calls.

### Tests

`tests/test_web_memory.py` (new, 4 tests) over the Streamlit panel's full-pipeline seed with a fake model: overview counts and rows, the switch (persisted; env override reported), inspectors, nominate → 409 on repeat → discard → nominatable again → delete, the skill lifecycle (invalid edit 400, valid edit + backup, promote, demote, fork a built-in → diff identical → prune), target ordering and match verdicts, consolidate guards, the upgrade job against a built-in with apply → fork + backup + consumed record, the consolidate job → `auto_<label>` provisional with `t2_consolidated` provenance, and a hostile/corrupt store. Web + memory suites: 119 pass. Full suite compared against main: no new failures (the 22 that differ are order-dependent live-test skips that pass in isolation on both branches).

### Live check (Bedrock, Opus 4.8)

On a seeded scratch store (two domains, a proven record, a 4-record variant group, three inbox streams plus a related same-session lesson, a hostile label, a corrupt file, a modified fork of the built-in `raman` skill), through the browser: switch persisted to `config.json`; bank inspector; group nomination staged 3 variants under one label; inbox inspector with its bank link; swept-label warning disabling consolidation until renamed; two-click discard; fork diff; invalid edit rejected, valid edit saved with backup; approve; suspend. Then with the real model: **consolidate** 4 records (a banked hot win, an error lesson, the user's feedback, and the related SNIP lesson pulled in from another label) → `auto_raman_dg` rewritten with `n_examples: 4`, every lesson present in the right section (clip window before fitting, area not height ratio, FWHM flag, SNIP iterations), backup kept, inbox group consumed; **upgrade** 3 variant records into the approved `voigt_baseline_rules` → preview in seconds, diff reviewed, proposal edited in the panel and the additivity check re-run (a destructive edit is flagged as "24% shorter", an additive one is clean), applied with backup, records consumed, bank rows nominatable again. The CLI (`scilink memory list/staged/status`) reads the same store identically.

### Docs

`react_web_ui.md`: Skills-tab bullet extended with the memory panel, the endpoint table, not-yet-ported list; `react_ui_roadmap.md`: item 6 marks the memory pipeline done.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cbf8e2980900`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
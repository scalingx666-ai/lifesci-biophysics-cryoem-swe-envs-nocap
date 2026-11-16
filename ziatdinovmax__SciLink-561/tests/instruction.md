# Issue

**Title:** Skills tab: upload custom skills, browse the catalog, view a skill

## Description

## Summary

The web UI has a **Skills** tab in every mode: drop one or more skill `.md` files to make them available to the agents for this session, and browse the built-in catalog — every bundle by domain with its one-line description and a viewer for the full text.

Persistent memory (graduated and auto-distilled skills under `~/.scilink`) is deliberately not in this PR; it is the next item and needs its own design.

## Technical description

- `scilink/server/skills_api.py` (new): `skill_catalog(agent)` from `loader.list_all_skills` plus each skill's frontmatter description via `load_skill` (48 files load in about 30 ms, so no cache), and the agent's `_custom_skills`; `skill_markdown(agent, domain, name)` (`custom` domain reads the session's registered file); `register_uploaded_skills` saves `.md` uploads under `<session>/custom_skills/` and calls the orchestrator's `register_skill` — a file that fails to register is still saved and reported, the rest proceed.
- `app.py`: `GET /sessions/{id}/skills`, `GET /sessions/{id}/skills/{domain}/{name}` (text/markdown), `POST /sessions/{id}/skills` (multipart). All four orchestrators have `register_skill`; an agent without it reports `skills_supported: false` and uploads return 400.
- `SkillsPanel.tsx` (new): dropzone limited to `.md`, registered-name chips, the custom list with a viewer, the catalog by domain with a filter that auto-expands matching domains, and a fixed-position markdown viewer that renders the YAML frontmatter as a caption (its `---` fence would otherwise turn the description into a title). `App.tsx`: Skills tab before MCP. `api.ts`: `SkillCatalog`, `skills`, `skillMarkdown`, `uploadSkills`.

### Tests

One new test in `tests/test_web_server.py`: catalog shape and labels, markdown content type and 404s for unknown built-in and custom names, an upload of a good, an empty and a non-`.md` file (one registered, two reported, file bytes on disk, catalog refreshed), custom markdown round-trip, and the no-`register_skill` agent. Suite: 45 pass. `tsc -b` clean.

### Verified in Chrome

A skill uploaded through the endpoint appears under Custom with its file name and a view link; the catalog lists 16 domains with 48 skills; Curve Fitting expands to seven rows with descriptions; the raman skill opens in the viewer.

### Docs

`react_web_ui.md`: Skills tab bullet, the three endpoints, not-yet-ported list updated.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a6ff89bd8e32`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
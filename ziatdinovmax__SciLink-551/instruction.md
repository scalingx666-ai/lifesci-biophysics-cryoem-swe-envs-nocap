# Issue

**Title:** Delegation tree in the sidebar; the detail tab becomes Telemetry

## Description

## Summary

The live delegation overview now sits in the left sidebar, exactly where Streamlit had it: a compact tree under the session section, visible the whole time you chat, with one line per delegation (status glyph, number, short label, context arrows), colored by status and counting elapsed time while a delegation runs. Clicking a row opens that delegation's full detail.

The detail view is kept and its tab is renamed **Telemetry**, which is where the per-agent tool sequence and worker histories will join it later.

## Technical description

- `webui/src/components/DelegationTree.tsx` (new): the sidebar tree, the web twin of Streamlit's `_meta_delegation_tree`. Groups by specialist in a fixed order, box-drawing prefixes for the branch structure, status classes for color, a 1 s ticker while any row is running, a bounded scroll box (260 px). Reads the same `DelegationView` the detail panel uses; no backend change, no new events.
- `Sidebar.tsx`: optional `delegations` and `onSelectDelegation` props; renders the tree in a section after the session block for meta sessions only.
- `App.tsx`: tab renamed `telemetry` (label "Telemetry"); the count badge moves off the tab since the tree carries it; a sidebar click sets a `focusDelegation` and switches to the tab.
- `DelegationsPanel.tsx`: new `focus` prop expands the requested row; docstring reframed as the Telemetry-tab detail.
- Styles for the tree appended to `app.css`.
- Docs: `react_web_ui.md` describes the two surfaces; roadmap wording adjusted.

Verified in Chrome against a resumed meta session with one completed delegation: the tree renders under the session section with the row in green, and clicking it opens Telemetry with the delegation expanded. `tsc -b` clean. The bundle is not committed (built at release).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0d5dffa5b534`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
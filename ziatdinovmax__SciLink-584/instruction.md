# Issue

**Title:** Web UI: smaller landing-page mode picker; readable code panes in light mode

## Description

## Summary

Two cosmetic fixes. The Mission Control / Analyze / Plan picker on the landing page was a size too large; it is now 15 px with tighter padding and the option rows 14 px. In light mode the Telemetry tab's Arguments and Result blocks (and the memory panel's diff) were unreadable because the dark code pane inherited the page's dark text; they now use the code-pane text color in both themes.

## Technical description

`webui/src/app.css` only: `.mode-trigger` 18 → 15 px, padding 12/20 → 8/16, min-width 300 → 260; `.mode-option` 15 → 14 px, padding 12/14 → 8/12; `.mode-emoji` 22 → 18 px; `.welcome .mode-desc` 15 → 14 px. `.tel-json` and `.mem-diff` gain `color: var(--code-text)` (the tokens keep code panes dark in both themes, so the text color must be explicit). Verified in Chrome in light mode: the expanded tool row shows its arguments and result; the mode picker at the new sizes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b023295cfded`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
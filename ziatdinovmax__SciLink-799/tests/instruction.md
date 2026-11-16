# Issue

**Title:** A grid in a plan's steps is shown as a table at the review gate

## Description

## What

Tables did not render in the plan-review gate (the UI panel that awaits human feedback on a proposed plan).

**Cause.** The planner prompt asks for a grid or gradient to be laid out as a markdown table inside `experimental_steps`, and the model writes each row as its own step entry. The gate's `steps` block numbered the header, the delimiter and every row as separate steps, and the web UI rendered each step as plain text, so the pipes showed raw. The markdown pipeline itself renders GFM tables fine.

**Fix.**
- `merge_table_rows` (`planning_agents/user_interface.py`) joins consecutive table-row entries into one markdown table, attached under the step that precedes it (after a banner or at the start it stands alone), and supplies the delimiter row when the author left it out, since without one nothing renders a table. A lone pipe-bearing entry stays a step. Used by the subject builder and the console printer; the ideation path indents the rows so the table stays inside its bullet.
- Web UI: each step renders through `MarkdownBody` (grid, bold, code, tilde-safe units), with small CSS for paragraphs and tables inside a step.
- Shell: a step's first line is numbered, what follows is Rich markdown.

## Checks

- New test for the merge, the supplied delimiter, the banner and lone-pipe cases, and the ideation indent; the subject, shell-widget, planning and ideation suites pass.
- `markdown_check.ts` gained a case for a step's grid rendering as a table; `tsc -b` clean.
- Looked at in the real `FeedbackPanel`: a fixture plan subject with a 4x3 annealing grid, mounted with the app's CSS and served by Vite, opened in Chrome. The grid is a bordered table under its step, the following steps keep their spacing, and there are no console errors. The shell widget and the console printer were rendered by hand and show the grid as a table too.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5424e29c1f63`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
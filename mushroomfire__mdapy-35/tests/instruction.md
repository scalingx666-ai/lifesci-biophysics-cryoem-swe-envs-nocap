# Issue

**Title:** Figures without matplotlib's names: plot.style options and an example

## Description

- `plot.style(*, font_size=None, line_width=None, colors=None, settings=None)`: the size of the text (tick labels and legends 6/7 of it), the width of the lines of data, the colors that lines and points take in turn (one or several), and any other setting of matplotlib by its name, ahead of the options. Keyword-only with defaults, so `style()` is unchanged; unknown settings or colors raise `InvalidParameter` with matplotlib's message, before the context starts.
- The API snapshot of 2.0.0, not yet released, is written again with these parameters, and the release notes of 2.0.0 name them.
- New example, Figures with MDAPY (`docs/examples/plotting.md`): a first figure, the look of a figure for a slide, anything else by its name, one line differently with the methods of the axes, several panels with the plots of MDAPY, saving.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f962c085fc1f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
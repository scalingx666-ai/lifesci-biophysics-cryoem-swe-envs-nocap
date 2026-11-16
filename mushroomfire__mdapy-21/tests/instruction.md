# Issue

**Title:** A legend beside the picture, a color for each line, and a ninth label color apart from the first

## Description

## What this changes

- `render(..., legend=...)` puts the legend in a column of its own at the left of the picture and frames the scene in the width that is left, so that a legend never covers atoms or the edges of the box. The picture keeps the size asked for; the column has the color of the background, or is transparent.
- `render(..., line_color=...)` takes one color, as before, or a color for each line, such as one for each atom whose path `trajectory_lines` gives.
- The ninth of `LABEL_COLORS` is brown. It was an orange 4 apart from the salmon of the first in OKLab (×100), so that the copies 0 and 8 of a picture looked alike; the brown is 18 from its nearest.

## How the results were checked

- The part of a picture beside the legend is, pixel for pixel, the picture of the same scene rendered in the narrower width.
- A longer name or a title gives a wider column; a legend wider than the picture still gives a picture of the size asked for; on a transparent background the column stays transparent around the text.
- Two lines in two colors give pixels of both colors; one color in a list of one is the same picture as the color itself; a list of the wrong length is refused.

## Checklist

- [x] `pytest` passes, and CI passes on Linux, macOS and Windows
- [x] `ruff check .` and `ruff format --check .` pass
- [x] Coverage is at least 95 % for each file changed
- [x] `mkdocs build --strict` passes

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4ab798834011`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
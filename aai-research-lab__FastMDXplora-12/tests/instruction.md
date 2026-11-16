# Issue

**Title:** Align Plot Fonts, Axes, and DimRed Colorbars for Slide Consistency

## Description

- sync cluster colorbar/fonts with axis sizing by capturing x/y tick sizes in `apply_slide_style` and updating `match_colorbar_font`
- standardize RMSF atom index axis to start at 0 with adaptive spacing
- force DBSCAN distance matrix axes to include the final frame tick (e.g., 500)
- normalize dimred (PCA/MDS/t-SNE) colorbars to 0–N frames and generate matching adaptive ticks

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `19ce06b7d3b9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
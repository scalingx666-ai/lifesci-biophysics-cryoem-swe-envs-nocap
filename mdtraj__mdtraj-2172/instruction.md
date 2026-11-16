# Issue

**Title:** Add bridge_anchors option to image_molecules

## Description

image_molecules keeps separate anchor molecules together with a single-closest-contact minimum-image heuristic, which picks the wrong periodic image when the assembled complex spans more than half the box along an axis, splitting elongated multi-chain complexes (e.g. protein dimers) across the periodic boundary.

The new bridge_anchors option treats all anchor molecules as a single connected complex: they are stitched together at their real-space interface contacts (measured on a made-whole reference) so make_whole reassembles the whole complex deterministically, bypassing the per-pair minimum-image heuristic. _sort_bonds now accepts extra_bonds so the bridges are folded into the existing spanning-tree make_whole path.

Defaults to False, so existing behaviour is unchanged.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `56616475fd12`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
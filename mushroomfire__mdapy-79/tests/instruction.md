# Issue

**Title:** DislocationLine.character: screw and edge along the lines

## Description

Builds on #78 (and #77, #76, #75), whose commits it contains until those merge.

`DislocationLine.character()` gives the angle between a line and its spatial Burgers vector at each point, in degrees from 0 to 90: 0 for screw and 90 for edge. The tangent at a point comes from the points before and after it. On a closed line it runs across the ends, a line through the box included; at a free end it is one-sided.

Tests against the angles the geometry fixes:

- Straight screw and edge dislocations of copper give 0° and 90° within 2°.
- Their Shockley partials give 30° and 60°, within 2° and 4°.
- A loop of 1/2[111] on (1-10) in iron follows the tangent of its circle, within 4° on average and 10° at most (its 15 points form a polygon). It runs from below 15° to above 75°.
- A line between two free surfaces gives finite angles from 0 to 90 at every point.

The DXA page gets a section "Screw and edge" with the 60° partials of its first example. The analysis tests and the docs tests pass locally (771).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `87d7b732ba76`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
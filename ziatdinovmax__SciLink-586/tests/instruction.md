# Issue

**Title:** Web UI: center the landing-page mode picker

## Description

## Summary

The Mission Control / Analyze / Plan picker on the landing page is now centered: the trigger's label is centered between its icon and caret, and the option rows in the open menu are centered rather than left-aligned. Verified in Chrome.

## Technical description

`webui/src/app.css` only: `.mode-trigger .mode-name` text-align left → center; `.mode-option` gains `justify-content: center` and text-align center.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `10904ecb6b7a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
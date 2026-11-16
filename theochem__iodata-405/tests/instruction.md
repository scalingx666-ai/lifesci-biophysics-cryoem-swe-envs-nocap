# Issue

**Title:** fix: raise LoadError for stray closing tags in WFX parser

## Description

## What

`parse_wfx` raised `KeyError: None` when a closing tag (a line starting with `</`) appeared outside any open section. This was reported in issue #383 where Orca-generated `.wfx` files caused a crash during loading.

## Root cause

The parser's first branch checked `line.startswith("<")` without excluding closing tags. A line like `</Primitive Exponents>` matched that branch (since `</` starts with `<`) when `section_start` was `None`, so it was treated as an opening tag. The `section_end` was computed as `<//Primitive Exponents>`. When the actual next closing tag arrived, the wrong-tag error fired — or in degenerate cases, content fell to the final `else` branch and did `data[None].append(line)`, producing `KeyError: None`.

## Fix

Two targeted changes to `parse_wfx` in `iodata/formats/wfx.py`:

1. Exclude closing tags from the opening-tag check: `line.startswith("<") and not line.startswith("</")`.
2. Split the closing-tag `elif` to first check `section_start is None` and raise `LoadError` with a clear message, then check whether the tag matches the expected closing tag.

Also guard the final `else` so non-empty, non-comment content outside any section raises `LoadError` rather than silently corrupting the `data` dict.

## Tests

- Added `iodata/test/data/h2o_stray_closing_tag.wfx`: minimal fixture with a stray `</Stray Closing Tag>` before any section opens.
- Added `test_parse_wfx_stray_closing_tag`: verifies `LoadError` is raised with an informative message instead of `KeyError`.
- All 52 `test_wfx.py` tests pass (51 existing + 1 new).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `10c71d590918`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
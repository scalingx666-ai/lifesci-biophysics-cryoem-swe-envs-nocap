# Issue

**Title:** read_file search: page through matches, say where they are, never claim exhaustiveness

## Description

## Summary

.

A `read_file` search that finds more than 40 matching lines used to hand back the first 40 by position with a soft aside, while the tool description promised "every matching line". On a 267-page PDF that meant the agent saw the front matter, believed it had read the document, and cited it. Now a capped search says plainly that it is truncated and partial, shows where in the file the remaining matches fall, and can be paged with `match_offset`; the descriptions in all four modes say the same thing and tell the model to narrow or page before citing.

## Technical description

### Engine (`scilink/utils/file_io.py`)

- `window_lines` / `read_file_content` take `match_offset` (1-based match rank). The search result carries structured `truncated`, `next_match_offset`, `shown`, `match_offset`, `match_lines` and `match_density`.
- Notice text on a capped search: `1033 matching line(s) in 22209 total; showing matches 1-40 of 1033 (lines 1-856). TRUNCATED: 993 more match(es), up to line 22026, are NOT shown — this is a partial result, not the document. Narrow the pattern to what you actually need, or continue with match_offset=41`, followed by `Where the matches are (equal line bands): lines 1-2221: 98 · lines 2222-4442: 132 · …`.
- Paging past the last match is explained (`match_offset=999 is past the last match (#200, line 996)`). A search under the cap is unchanged apart from the new fields; the head/offset/tail modes are untouched.

### Tools

All four `read_file` wrappers (planning, analysis, simulation, meta) take `match_offset`, forward it, and carry the same `search` and `match_offset` descriptions: up to 40 per call, total count and location, `truncated` / `next_match_offset`, and "a truncated search has NOT read the document — narrow the pattern, or page with match_offset, before citing it". "Returns every matching line" is gone from all of them.

### Tests

`tests/test_read_file_search_paging.py`: first page truncated with next page and a 10-band density map; paging walks every match exactly once and the last page is untruncated; paging past the end is explained; an under-cap search has no density noise; `match_offset` rides through `read_file_content`; every orchestrator describes the cap and forwards the parameter. The two older "showing the first 40" assertions were updated. `tests/test_read_file_offset_and_index.py` pointed at another user's checkout and a session that does not exist here; it now reconstructs its fixture, gives the exec-lifted wrapper its package identity for the relative import, and no longer calls `sys.exit` at import (which aborted pytest collection). 38 read_file / file_io tests pass.

### Live verification

Engine on the real documents: the DOE Critical Materials Assessment (267 pages, 22,209 extracted lines) with the issue's own pattern gives 1033 matches, `truncated: true`, `next_match_offset: 41`, a density map with hits in every tenth of the file, page 2 continuing at line 877, and the narrowed pattern `magnesium.*(critical|criticality)` landing on line 969 ("nickel, platinum, magnesium, SiC, and praseodymium become critical"). A USGS Mineral Commodity Summaries PDF gives 34 matches, untruncated.

Agent on Bedrock Opus 4.8 (planning orchestrator, both PDFs in the data folder, asked to start with the broad search): the model ran the broad search (1033 matches, truncated), then read around the last hit shown (`offset=857`) instead of stopping, and answered with a verbatim quote that really is at lines 855-857 ("The 2019 CMS report found seven materials to be critical in the medium term … and magnesium (Mg)") plus the correct USGS production figure with its line. It did not page or narrow further, so it reported the 2019 finding rather than the 2023 assessment's own statement at line 969; the truncation was visible to it and the citation is real, which is the failure the issue describes no longer happening.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3f7706dd840f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Web UI: syntax highlighting for source files and fenced code

## Description

.

## Summary

Generated scripts, skill snippets and any fenced code in the web UI rendered as plain text. Source files opened in the file explorer and fenced code blocks in markdown (skill previews, reports, chat replies) are now syntax-highlighted for the common languages, with colors that follow the app's design tokens. Everything else in those views is unchanged: tables, images, inline file links and the plain-text fallback for unknown file types.

## Technical description

- `webui/src/highlight.ts`: highlight.js core (11.12) with python, javascript, typescript, json, bash, yaml, ini (toml), markdown, xml (html), css, diff and plaintext registered; extension → language map; fence-tag aliases resolved to the canonical language; `highlightCode` returns escaped text for unknown/plaintext languages or inputs over 120k characters and never throws.
- `CodeBlock.tsx`: `CodeBlock` (the file explorer's text preview and the markdown Source view, keeping the `text-preview` pane class) and `HighlightedCode` (the `<code>` inside a react-markdown fence).
- `MarkdownBody.tsx`: the `code` component now always exists; fenced/multi-line code is highlighted, inline code keeps the file-link click behaviour.
- `FilePreview.tsx`: `TextPreview` takes a `language` (from the extension; `json` for the pretty-printed JSON path); the markdown Source view highlights as markdown.
- `tokens.css` / `app.css`: `--hl-*` palette tokens and the `.hljs-*` class mapping; `code.hljs` is transparent so the pane keeps its surface (the panes are dark in both themes by design).
- `npm run check:highlight` (`scripts/highlight_check.ts`): mapping, tokenization per language, escaping, oversized-input cutoff, script-tag escaping. `tsc -b` and `vite build` clean; bundle 778 kB (gzip 238 kB).

### Verified in Chrome (resumed session on the test server)

Python `trend_analysis.py` in the file explorer highlighted (345 tokens); `analysis_results.json` highlighted (153 tokens, 34 keys); the EPR skill preview's python fence highlighted (81 tokens); light theme keeps the dark pane with readable tokens; a CSV still renders as a table; inline file links in chat still work.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9bdcd8571bb1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
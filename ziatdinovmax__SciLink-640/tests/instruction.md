# Issue

**Title:** Web UI: currency dollars are text, not inline math

## Description

## Summary

.

Dollar signs in reports and chat answers were being read as LaTeX delimiters: everything between two prices ("~$0.79/m³ … $300/t") turned into serif-italic math, and the tilde meant as "approximately" came out as red strikethrough or a literal `\~`. Now a single `$` is just a dollar sign in the chat, in document previews, in report cards, and in the Streamlit UI. Display math written as `$$ … $$` still renders as a formula.

## Technical description

**Cause.** `remark-math` treats single `$…$` as inline math by default. The tilde escape then split the text on those mis-detected spans, so tildes inside a "currency span" were left raw for `remark-gfm` to turn into strikethrough, while others double-escaped to a literal `\~`.

**Changes.**

- `webui/src/markdown_text.ts` (new, JSX-free): the shared remark plugin set with `singleDollarTextMath: false` (`remark-math` 6 / `micromark-extension-math` support it), `escapeTildes` splitting only on `$$…$$`, and `demoteHeadings` moved here. `MarkdownBody` imports and re-exports them, so all seven MarkdownBody call sites render through the same set.
- `FilePreview` and the markdown report card pass `escapeTilde` like the chat bubble, aligning the three agent-authored surfaces.
- `webui/scripts/markdown_check.ts` (`npm run check:markdown`) renders through the real react-markdown pipeline: currency stays text with all dollar signs present, no `<del>` from prose tildes, `$$` display math still goes through KaTeX, explicit `~~text~~` still strikes through. The same check on the old plugin set shows KaTeX output and a single surviving `$`.
- Streamlit twin: `st.markdown` also typesets `$…$`, so `_escape_tildes` now escapes single dollars as `\$` outside `$$…$$` as well.
- Audit: the HTML and PDF report generators render no math (no KaTeX/MathJax and no math extension), so exported reports were never affected.

**Trade-off.** Single-dollar inline LaTeX no longer renders; nothing in the app writes it on purpose, and display math is unchanged.

**Verification.** `tsc` + `vite build` clean; all 11 markdown checks pass. Live in Chrome against `scilink serve` with a Bedrock plan session: the model's verbatim currency answer renders with three literal dollar signs, tildes intact, no KaTeX and no strikethrough in the bubble; the expanded report card shows the currency prose as text and exactly one KaTeX element, the `$$` fit formula.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7837fd484828`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
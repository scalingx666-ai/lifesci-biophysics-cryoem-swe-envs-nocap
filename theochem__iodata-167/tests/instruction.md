# Issue

**Title:** Add contents table for supported file type

## Description

Enhancement: Add `gen_formats_tab.py` to generate a rst-format
table file `format_tab.inc`. This file is then included in the
new added page `supported_format.rst`
1. Add format and properties supported table
2. Add a temperary page for exhibiting the table
3. Add the autogen script to gen_docs.sh

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ace1ad9a2452`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
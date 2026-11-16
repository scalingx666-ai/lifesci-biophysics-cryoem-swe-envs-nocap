# Issue

**Title:** Document OFFXML string sources for ForceField

## Description

Documents that `ForceField()`, `ForceField.parse_sources()` and `ForceField.parse_smirnoff_from_source()` accept a string containing OFFXML content, in addition to file paths and file-like objects. As discussed in #2021, a string that does not resolve to a file name falls through to `ParameterIOHandler.parse_string()` and this has been de facto public API since the beginning, but all three `sources`/`source` docstrings described only files.

The `parse_sources` docstring also now says that a bare string is iterated character by character (`iter(str)` succeeds), so a single string source has to be wrapped in a list; `ForceField(<string>)` is unaffected because `*sources` already arrives as a tuple. This came out of checking each documented statement against the toolkit:

```
ForceField(<offxml string>)                              -> loads
ForceField().parse_sources([<offxml string>])            -> loads
ForceField().parse_sources(<offxml string>)              -> "Source '<' could not be read"
ForceField().parse_smirnoff_from_source(<offxml string>) -> {'SMIRNOFF': ...}
ForceField("openff-2.0.0.offxml")                        -> file-name lookup unchanged
ForceField("openff-2.0.0.offxml", <offxml string>)       -> mixed sources load
```

The `__init__` entry is also reflowed into a bullet list, per the whitespace comment on the issue. `ruff check` passes (one rewrapped line was over the 119-character limit) and `pytest openff/toolkit/_tests/test_forcefield.py -k "string or parse_sources or xml"` gives 164 passed, 4 skipped. Docstring-only change, so no new tests. The release-history entry references this PR's number.

- [x] Tag issue being addressed
- [ ] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests) (docstring-only change)
- [x] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://docs.openforcefield.org/projects/toolkit/en/stable/users/developing.html#style-guide) codebase
- [x] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

See our [AI Policy](https://github.com/openforcefield/openff-toolkit/blob/main/AI_POLICY.md). Please indicate whether LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution:
- [ ] No
- [x] Yes

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ef074d58a2e7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
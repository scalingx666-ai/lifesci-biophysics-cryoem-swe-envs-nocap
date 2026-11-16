# Issue

**Title:** Skip empty lines in an FCIDUMP file

## Description

A WFX or FCIDUMP file with an empty line at the end could not be loaded. A trailing empty line is easy to get, for example from an editor that adds one or from a script that appends to the file.

- WFX: an empty line between sections (outside a section) was added to `data[None]` and gave a `KeyError: None` from `parse_wfx`. Empty lines between sections are now skipped, the same way comment lines already are. Inside a section an empty line is still data.
- FCIDUMP: an empty line among the integrals gave "Expecting 5 fields on each data line in FCIDUMP, got 0", and an empty line in the header would have been an `IndexError` on `words[0]`. Empty lines are now skipped in both places.

I found this by loading every file in `iodata/test/data` again after adding empty lines or trailing spaces at the end, and with Windows line endings and a missing final line break. The Windows (`\r\n`) and old Mac (`\r`) line endings worked everywhere. The other failures are in different formats and I will send them separately.

Tests: `test_load_empty_lines_between_sections_and_at_the_end` for three WFX files and `test_load_fcidump_empty_lines` for both FCIDUMP files. They write a copy of the test file with the extra empty lines and check that it loads with the same values. The five tests fail without the change, and the WFX and FCIDUMP tests pass (60 tests).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a9a56d4a950c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
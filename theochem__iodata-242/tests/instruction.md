# Issue

**Title:** Use string `.format` method instead of `Template` module in `write_input`

## Description

This replaces the Template module with the format string method, as discussed in #241. 

I felt that it would be easier to unpack the `fields` dict into the `format` method, so that the template looks like
`#n {lot}/{obasis_name} {run_type}` and not `#n {fields[lot]}/{fields[obasis_name]} {fields[run_type]}`. I don't think that will cause any problems. 

I also added another example in the docs, to show that it's possible to read a template from file, in case someone is hoping to do that.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `facebbdc31d6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
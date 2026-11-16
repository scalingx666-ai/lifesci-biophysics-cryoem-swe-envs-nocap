# Issue

**Title:** AMBER parser had a "too strict" regex to extract values

## Description

This PR addresses issue #272. 
This issue exposed a nasty bug inside the regex pattern we used to extract sections in the amber output file.

we are trying to match a sequence of type

filed = int/float

extracting the int/float corresponding to the field. Up till now, the pattern was 
```python
fr' {field}\s+=\s+(\*+|{_FP_RE}|\d+)'
```
were `field` was the string we want to extract the value for, and `_FP_RE` is defined to extract the float/int after the equal.

The problem arises when the field and/or the value are not separated from the `=` sign by a space (as was the case in the issue). It's strange this didn't appear before!

I just changed the `+` in `\s+=\s+` to `*`.

Thinking about it, this should not break anything else, but I can't be 100% sure and the tests should catch any "macro" problems eventually introduced with this PR.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6e19d4edb944`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
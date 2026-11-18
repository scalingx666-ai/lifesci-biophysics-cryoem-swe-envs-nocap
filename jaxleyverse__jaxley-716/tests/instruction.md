# Issue

**Title:** Added Example Usage Documentation

## Description

I have a few questions before we can merge this PR...

- Should I add jaxley.Module to top of API reference?
- Is the style of the example usage using ">>>" good, or should I remove it?
- Should Module.scope() and Module.set_scope() include Network examples as well?
- Should you really be able to do .comps on Networks and Cells, or just Branches?
  Because it is not working the way it intuitively should.
- How to I get diffuse() to show up on the docs?
  - Why does diffuse() have the only_allow_module() decorator?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6c8c9c00fc26`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
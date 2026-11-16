# Issue

**Title:** Remove all instances of `&` in SMARTS defs from the latest additions to `oplsaa.xml`

## Description

### PR Summary:

In  using the `&` (a1 and e2 (high precedence)) in some atom type definitions seems to break the xml parser in forcefield utilities and is causing the GMSO CI to fail. I removed them in this PR, and it doesn't change the atom typing results for the OPLS validation tests. 

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4db6e4bae77f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
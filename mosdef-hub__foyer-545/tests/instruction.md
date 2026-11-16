# Issue

**Title:** Do not use 'old' OpenFF Toolkit API

## Description

### PR Summary:

A change I made to the toolkit () involved changing how attributes are looked up, which made this check

```
        uses_old_api = hasattr(Topology(), "_topology_molecules")
```

no longer produce the same behavior as when I wrote it. With 0.14.4, released last night, this line erroneously evaluates to `True`. There would be a few ways to fix this, but I'm proposing to just remove the dual support. We are no longer supporting the "old" (< 0.11) versions of the toolkit. Note, of course, that this change wouldn't affect any released version of Foyer.

I also noticed tests aren't run in parallel - it's a small and straightforward fix to get CI to use the multiple cores that [GHA provides 
](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners/about-github-hosted-runners#supported-runners-and-hardware-resources). I'm lazy.

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [x] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d2608b78608f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
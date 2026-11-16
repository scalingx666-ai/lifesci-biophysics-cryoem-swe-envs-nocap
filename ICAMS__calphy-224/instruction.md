# Issue

**Title:** Make queue options a dict and write nicely

## Description

Previously the 'options' key in the 'queue' block was expected to be a plain list of strings, like so

```
options:
 - "--account=myname"
```

Now it accepts

```
options:
 acount: myname
```

Both are different from the previously documented style

```
options:
 - acount: myname
```

which yields lists of dicts and messed up the writing logic.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `333ad4da0ac6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
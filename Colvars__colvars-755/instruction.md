# Issue

**Title:** Fix indentation of 'Init CVC' message in standard output

## Description

A tiny change, move a `cvm::increase_depth();` instruction up so that the message "initializing component" lines up with the parameters below, consistently with all other objects.

Before the change:
```
colvars:   Initializing a new collective variable.
colvars:   # name = "d"
colvars:   Initializing a new "distance" component.
colvars:     # name = "" [default]
```

After:
```
colvars:   Initializing a new collective variable.
colvars:   # name = "d"
colvars:     Initializing a new "distance" component.
colvars:     # name = "" [default]
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d20c946b011f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Avoid shell=True when invoking PACKMOL

## Description

## Summary

PACKMOL is currently launched through a shell with an interpolated temp path:

```python
proc = Popen(
    f"{PACKMOL} < {packmol_inp.name}",
    stdin=PIPE, stdout=PIPE, stderr=PIPE,
    universal_newlines=True, shell=True,
)
```

Two problems:

1. **Shell-injection anti-pattern** — building a shell command line by interpolating a path into an f-string with `shell=True`.
2. **Breaks on spaces / shell metacharacters** — if the temp file path contains a space (e.g. `TMPDIR` under a user directory with spaces, common on macOS/Windows), the `<` redirection targets the wrong token and PACKMOL fails.

The shell was only being used for the `<` input redirection; `stdin=PIPE` was set but nothing was ever written to it.

## Fix

Feed the input file directly as `stdin` and pass the executable as an argument list, dropping the shell. `PACKMOL` is a resolved absolute path from `shutil.which("packmol")`, so no shell word-splitting is needed.

```python
with open(packmol_inp.name) as inp_file:
    proc = Popen(
        [PACKMOL],
        stdin=inp_file, stdout=PIPE, stderr=PIPE,
        universal_newlines=True,
    )
    out, err = proc.communicate()
```

Behavior is unchanged (PACKMOL reads its config from stdin exactly as before), and the call is now injection-safe and robust to paths with spaces.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `51e6c9b4f494`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
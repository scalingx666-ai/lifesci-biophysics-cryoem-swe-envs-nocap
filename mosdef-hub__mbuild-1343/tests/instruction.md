# Issue

**Title:** Use shutil.copyfile instead of os.system("cp") in packing (Windows portability)

## Description

In `_run_packmol` (`mbuild/packing.py`), two copies shell out to `cp` via `os.system`. `cp` is a Unix-only command, but mBuild is distributed on conda-forge for **win-64**, and `_check_packmol` right below already branches on `sys.platform.startswith("win")`. On Windows both calls fail, which breaks:

- the "imperfect packing" fallback that copies `<name>_FORCED` over the result, and
- the copy of the packed file to the user-provided `temp_file`.

They also invoke a shell with interpolated filenames unnecessarily (breaks on paths containing spaces).

### Change
Replace both `os.system("cp ...")` calls with `shutil.copyfile(...)` — portable, shell-free, and already used elsewhere in this module (`shutil` is imported at the top). Also drops a no-op single-argument `os.path.join(temp_file)`.

No behavior change on Unix; fixes the code path on Windows. Happy to add a regression test if desired.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `293124c14542`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
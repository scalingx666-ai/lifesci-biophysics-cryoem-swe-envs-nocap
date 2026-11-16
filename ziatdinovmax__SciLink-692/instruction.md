# Issue

**Title:** Windows: a script and everything it starts live in one Job object

## Description

> Stacked on #684 (process groups on POSIX). Review after #684. **Not run on Windows**: there is no Windows machine or CI here. See "Verification".

## Summary

On macOS and Linux, #684 makes a script's timeout, a Stop, or SciLink exiting end the script together with every program it started. On Windows only the script itself was ended, and anything it had started kept running. Now Windows gets the same treatment, through the mechanism Windows provides for it, Job objects. If SciLink itself crashes on Windows, the operating system ends the scripts too.

## Technical description

**The change** (`scilink/executors.py`).
- **`_WindowsJob.attach`:** each script, and the warm executor's worker, is assigned right after it starts to a Job object created with `JOB_OBJECT_LIMIT_KILL_ON_JOB_CLOSE`. The steps are `CreateJobObjectW`, then `SetInformationJobObject(JobObjectExtendedLimitInformation)`, then `AssignProcessToJobObject(job, proc._handle)`.
- **Timeout and Stop:** `_kill_process_tree` sees `proc._scilink_job` and calls `TerminateJobObject`, which ends the whole tree at once, then closes the handle.
- **Normal exit:** `_end_leftover_group` terminates the job too, and the warm executor's `close()` calls it.
- **SciLink itself dying:** its handle closes, and kill-on-close makes Windows end the tree. This is the case the POSIX side needs SIGHUP/SIGTERM handlers for.
- **Failure handling:** any Win32 failure (no ctypes, a nested job refused on an old Windows) closes the unused handle and leaves the process unassigned, and the caller falls back to killing the process alone, the behaviour before this PR.
- **Signatures:** the kernel32 signatures are declared (`HANDLE`, `BOOL`, `DWORD`), so a 64-bit handle is not truncated to a C int.

**Known limit.** A child created in the first instants of interpreter startup, before the assignment, would escape the job. A generated script starts nothing that early. `CREATE_SUSPENDED` plus a resume would close that window at the cost of more Win32 surface; it is left out.

**Verification: `tests/test_windows_job_objects.py`.**
- **Structure layout:** `JOBOBJECT_EXTENDED_LIMIT_INFORMATION` is 144 bytes, its basic part 64, and `LimitFlags` sits at offset 16. These are the Win32 x64 values, and the layout is identical on any LP64 host.
- **Call sequence, through a fake `kernel32`:** create, then set kill-on-close (`0x2000`, class 9, the correct size), then assign the process handle.
- **Timeout/Stop and leftovers:** both terminate the job and close the handle, and the fake's terminate ends the real process.
- **A refused assignment** falls back to killing the process alone, without leaking the job handle.
- **POSIX never touches the Win32 path.**

The existing executor, warm-executor and Stop tests pass. **What only a Windows run can confirm** is that the real `kernel32` calls succeed in a nested-job environment such as a terminal, a CI runner or a container, and that `TerminateJobObject` ends a pool that `multiprocessing` started. A one-off check on a Windows machine is worth doing before relying on it.

- Full offline suite: no failures that `main` does not have.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `21a0253476c9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
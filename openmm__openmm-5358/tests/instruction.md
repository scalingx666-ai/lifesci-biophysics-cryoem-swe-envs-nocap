# Issue

**Title:** Support building the Python extension against a DESTDIR-staged install

## Description

setup.py links openmm._openmm against OPENMM_LIB_PATH, which is always the final, real installation directory -- correct for the runtime rpath, but wrong as the linker's search path in any packaging workflow that stages a build into a temporary root (DESTDIR) before the real system install, since the just-built C++ libraries only exist under DESTDIR + OPENMM_LIB_PATH at that point, not OPENMM_LIB_PATH itself. This is the normal pattern used by most Linux distro packaging (RPM, deb, etc.) via "make install DESTDIR=...", and currently fails there with "cannot find -lOpenMM" (confirmed with a from-scratch build).

Search DESTDIR + OPENMM_LIB_PATH (empty DESTDIR is a no-op, so this is unchanged when not building via a staged install) while leaving the extension's runtime_library_dirs -- and the existing Darwin -rpath, which already did this correctly -- pointing at the clean OPENMM_LIB_PATH, since DESTDIR never exists on the system the extension actually runs on.

Verified with a full two-pass DESTDIR build (matching the pattern OpenMM's own docs describe: build+install the C++ libraries, then build the Python wrapper referencing the now-installed libraries): fails with "cannot find -lOpenMM" without this change, builds and installs cleanly with it, and the resulting extension's RUNPATH correctly points at the real (non-DESTDIR) OPENMM_LIB_PATH either way.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1d716de3b510`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
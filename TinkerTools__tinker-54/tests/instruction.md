# Issue

**Title:** Add CMake build system

## Description

This PR adds an rudimentary version of a CMake buildsystem.

CMake is cross-platform and hence works on Linux, Mac OS and Windows.

The `CMakeLists.txt` is very human-readable and easy to extend.
I haven't added the GUI compile yet.

This CMake system is used in  to build tinker inside spack.
Not sure where the documentation for spack install should go, but in short:
```
git clone https://github.com/spack/spack.git
. spack/share/spack/setup-env.sh
spack install tinker
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9c74c1252e70`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
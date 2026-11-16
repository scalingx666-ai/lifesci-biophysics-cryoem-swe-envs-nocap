# Issue

**Title:** Update CMakeLists.txt for optimisation and cross-platform

## Description

I have updated the CMake file.

The previous version explicitly set build type and compiler flags independent of the user's settings and available compiler. Also, the compiler flags were overridden such that the "Release" mode didn't contain any optimisation flags.

This version sets Release version only when nothing else is specified, and adds the compiler flags from the previous version only if the user is using the GNU compiler (gfortran). The CMake defaults are otherwise not touched.

With this change, packmol should work on any operating system and with any Fortran compiler straight out of the box.

I tested this version with GNU and Intel compilers.

The build instructions are the usual
```
cd packmol
mkdir build
cd build
cmake ..
make -j$(nproc)
```

_Thank you very much for this software - it has been most useful in my molecular dynamics work!_

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2dd1cb300889`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
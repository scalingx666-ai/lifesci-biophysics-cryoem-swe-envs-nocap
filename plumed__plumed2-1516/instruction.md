# Issue

**Title:** Windows native build: CMake-based build system (MSVC, static kernel)

## Description

Adds a CMake-based build system that compiles PLUMED natively on Windows with MSVC, discussed in #934 (and long-standing requests #572, #855). Per @Iximiel's suggestion, this is kept as a separate, self-contained contribution.

**Contents:**
- `CMakeLists.txt`: builds all 'always' + 'default-on' modules as a static kernel library (no dlopen required on Windows)
- `cmake-msvc/PlumedGenerate.cmake`: CMake-native reimplementation of the build-time generators (makecmd, makeModuleMap, xxd, update-version.sh, Config.inc rules) — no external interpreter (python/awk/sed) needed
- `cmake-msvc/PlumedInstall.cmake`: install rules mirroring the essential parts of `make install` for the static-kernel build
- `.github/workflows/windows.yml`: minimal Windows CI (build + `plumed info --version` + smoke test), green on windows-latest: https://github.com/huyang-md/plumed2/actions/runs/39190713462

**Also includes 4 cross-platform fixes required to compile the master branch with MSVC** (each as a separate commit):
- `tools/NeighborList.cpp`: stray 0x1A (Ctrl-Z) byte treated as EOF by MSVC
- matrixtools: ill-formed `MatrixTimesVectorBase<Vector>` instantiation
- secondarystructure: Empty specialization members guarded with `if constexpr` (MSVC eagerly instantiates them)
- function: declare the `Custom writeInGraph()` specializations (ODR)

All checks green (22/22 including the upstream CI matrix and codecheck). The autoconf+make build is untouched; this lives alongside it as the Windows path.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1e03000ea9ff`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
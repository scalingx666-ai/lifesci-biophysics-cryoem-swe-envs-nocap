# Issue

**Title:** MSVC (Windows native) build: 6 compatibility fixes

## Description

(the 6 source fixes; the CMake build system is discussed separately there, see also the thread with @Iximiel).

This PR makes PLUMED 2.10.1 compile with MSVC (native Windows, no WSL/MinGW):

1. **lepton/Operation.h**: replace the `1.0/0.0` literal with `std::numeric_limits<double>::infinity()` — MSVC rejects the literal at compile time (C2124).
2. **tools/Tools.cpp** + **bias/MetaD.cpp**: `std::filesystem::path` is `wchar_t`-based on Windows; add explicit `.string()` conversions.
3. **bias/PBMetaD.cpp**: fix a full-width quote typo inside a string literal.
4. **config/Config.inc.in**: guard `dlfcn.h` with `__PLUMED_HAS_DLOPEN`; add missing `return` in `getLibraryPath()` when DLADDR is unavailable.
5. **main/main.cpp**: fix int→size_t narrowing in a braced init list.
6. **ves/WaveletGrid.h**: add missing `#include <string>`.

Bonus (same theme, cltool framework): **core/CLToolMain.cpp** catches PLUMED exceptions from `cl->main()` so user-input errors terminate with a message and nonzero status instead of an uncaught-exception abort (0xc0000409).

All changes are no-ops for Linux/macOS (GCC/Clang accept the patched code unchanged; same semantics).

Verified end-to-end on Windows: full MSVC build completes; the static kernel links into a native GROMACS 2024.4 build; well-tempered metadynamics produces correct COLVAR/HILLS output. Build/verification details are documented in our repository (link available on request).

One commit per changed file, as suggested by @Iximiel. cc @carlocamilloni

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `75d6916f63a9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
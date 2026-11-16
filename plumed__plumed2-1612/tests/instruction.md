# Issue

**Title:** Windows wheels

## Description

# Windows wheels: plan and status (pip packaging plan, step 5)

Started 2026-11-10 from master e0e36e29. PRs: A (shared kernel, C/C++), B (Python interface on Windows), parity (#1609: asmjit, stack, stack traces, UTF-8 names; see claude/windows-parity-open-points.md), C (Windows wheels).

## Status
- PR A (#1604) merged (c773cbcbe). PR B (#1608) merged (2317b9a3d). windows-parity (#1609) merged (8f7e2b602).
- **PR C (branch windows-wheels, 3 commits on master e704b4395, 2026-11-11)**: 9c5c8e159 CMake, f7ed32f7a python, 720da2fa6 CI. Delivered as `windows-wheels.bundle` (chat outputs; the Mac was unreachable, so it is NOT in `~/Codes/plumed2/Claude outputs/` yet). Carlo: `git fetch windows-wheels.bundle windows-wheels:windows-wheels`, push, open the PR. Nothing of the Windows wheel could be run in the cloud: the first python-package.yml run on windows-latest is the real test.

## PR C: what it does
1. **CMake** (9c5c8e159): option PLUMED_PIP_BUNDLE (needs PLUMED_SHARED_KERNEL) -> define __PLUMED_PIP_BUNDLE and Config.inc generated as installed (generate.cmake gets -DINSTALLED -DPREFIX; root=<prefix>/lib/plumed, includedir=<prefix>/include, htmldir=<prefix>/share/doc/plumed). Removed the forced /MD in Release (runtime follows CMAKE_MSVC_RUNTIME_LIBRARY, default /MD unchanged). /MP with Visual Studio generators. Install now also: lib/plumed/src/config/config.txt, vim/templates + vim/scripts.vim, and an install(CODE) running the installed plumed `--no-mpi gen_vim --outdir lib/plumed/vim --template-dir <src>/vim/templates` and `gen_json > lib/plumed/json/syntax.json` (FATAL_ERROR if they fail). This runs also in the windows-shared-kernel CI installs.
2. **build_backend.py** (f7ed32f7a): `_compile_runtime_cmake` on win32: cmake (pip build requirement `cmake>=3.18; sys_platform == 'win32'`) with the default generator (newest Visual Studio, no dev prompt), -DCMAKE_BUILD_TYPE=Release -DPLUMED_SHARED_KERNEL=ON -DPLUMED_PIP_BUNDLE=ON -DCMAKE_MSVC_RUNTIME_LIBRARY=MultiThreaded (/MT: no msvcp140/vcruntime dependency, so no delvewheel needed; kernel API is C only) -DCMAKE_INSTALL_PREFIX=C:/plumed_runtime (compiled-in fallback), build --config Release --parallel <=4, install --prefix <tmp>/install, then `_stage_runtime` + `_finish_runtime` (shared with configure: licenses, build-info "Build system: CMake", no-symlink check). Temp dir with ignore_cleanup_errors (MSBuild processes). BUILD_INPUT_FILES += CMakeLists.txt, BUILD_INPUT_DIRS += cmake-msvc (also in the sdist and in the cache key, all platforms). src/include/plumed link not created on win32.
3. **pyproject**: skip *-win_arm64; [tool.cibuildwheel.windows] environment with PLUMED_RUNTIME_CACHE=C:/plumed-runtime-cache; test-command adds test_cmd_types, test_unicode_paths, test_hdf5; test-requires numpy, pandas, h5py with `python_version < '3.15'`; PIP_ONLY_BINARY=numpy,pandas,h5py in all environments.
4. **Tests**: test_runtime_resources and test_bundled_wheel have WINDOWS branches (kernel in bin, plumed.exe; no compile_options.sh/Template.cpp/main.cpp/modulefile/bash completion/plumed-patch/libplumedWrapper; skip LOAD, mklib, `plumed config show`, native client; temp dirs with ignore_cleanup_errors for copied DLLs; manual --vim output CRLF->LF; PE import table parser `windows_imports` forbids msvcp/vcruntime/concrt/api-ms-win-crt/mpi/blas... in plumedKernel.dll and plumed.exe). test_hdf5 skips without pandas/numpy. _inputbuilder reads the kernel log and Vim files as UTF-8.
5. **CI** (720da2fa6): python-package.yml matrix + windows-latest AMD64, `defaults.run.shell: bash`, PROJECT_DIR as native path via python os.getcwd(); windows.yml windows-shared-kernel installs h5py.
6. README.rst and CHANGES (Windows wheels, no LOAD/mklib/shell tools there).

## Checks done in the cloud (Linux)
- Configure/make wheel with the refactored backend: all wheel tests pass (test_input_builder, test_io, test_runtime_resources, test_cmd_types, test_unicode_paths, test_hdf5 also with h5py, test_bundled_wheel with the source tree).
- `_compile_runtime_cmake` run on Linux (CXXFLAGS=-D__PLUMED_HAS_DLADDR so the bundle lookup works without GetModuleFileNameW): runtime staged (bin/plumed, lib/libplumedKernel.so, lib/plumed/{scripts,patches,vim/{help(284),syntax,templates,scripts.vim},json/syntax.json,src/config/config.txt,src/lib/Plumed.inc,build-info.txt}, include/plumed); `plumed info --root/--include-dir` resolve under the runtime; wheel built with setuptools from it passes check_kernel, external kernel, json==vim, CLI json, metadata errors, vim fallback, vim generation, check_actions (208 actions, 19 tools), test_cmd_types, test_unicode_paths, test_io, test_input_builder, discovery/overrides of test_runtime_resources.
- ruff: no new findings.

## What to watch in the first Windows wheel run
- CMake VS generator found inside cibuildwheel's isolated build env (cmake from pip); build time with /MP (kernel built once, cached for the 5 Pythons).
- /MT for kernel and plumed.exe (CMAKE_MSVC_RUNTIME_LIBRARY needs policy CMP0091, NEW with cmake_minimum_required 3.18).
- install(CODE) gen_vim/gen_json with plumed.exe; json written by plumed.exe stdout in text mode (CRLF, harmless).
- Copies of plumedKernel.dll with the same base name loaded from different paths (alternate package test); DLL temp dirs.
- test_hdf5 with h5py on Windows (also in windows-shared-kernel); pandas/h5py binaries for each Python.
- Known: Plumed.inc/Plumed.cmake of the CMake install point to temporary build paths ("static installation" format) — meaningless in the wheel (as /plumed_runtime paths in the Linux wheel); all headers of all modules are installed (CMake installs every src/*/*.h).

## Background
- One package `plumed`: Cython extension `plumed._plumed`, pure-Python helpers, private prefix `plumed/runtime/{bin,lib,lib/plumed,include/plumed,share}` mirroring `make install`. Kernel resources found relative to the kernel only under `__PLUMED_PIP_BUNDLE` (src/config/Config.inc.in; Windows: GetModuleFileNameW, candidates bin/plumed and <prefix>/lib/plumed with scripts/).
- CodeQL results are post-filtered by .github/codeql/filter-sarif.py (documented, line-matched exclusions). Use it for by-design alerts instead of manual dismissals.
- Lessons: MSVC reads BOM-less sources in the ANSI code page unless /utf-8 (keep test sources ASCII); LoadLibraryEx searches relative paths in the DLL search path, not the cwd.

## After PR C (agreed 2026-11-11)
A list of proposals to improve all wheels: HDF5, chemfiles, zlib, `plumed config` in C++, LOAD of prebuilt DLL plugins on Windows (then .cpp), Subprocess on Windows, ARM64. Out of scope: MPI (MS-MPI), OpenMP (decided: no OpenMP in wheels).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `787007569bd8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Windows shared kernel

## Description

PR A: shared kernel on Windows (C/C++ only)
CMake option PLUMED_SHARED_KERNEL (default OFF on Windows for now): add_library(plumedKernel SHARED ...) from the same sources; export only the C API (__declspec(dllexport) macro on plumed_symbol_table, plumed_plumedmain_* and the other extern "C" entry points in PlumedMainInitializer.cpp, or a .def file). No WINDOWS_EXPORT_ALL_SYMBOLS (data symbols, size).
plumed.exe linked to the DLL through the C API (main.cpp + wrapper), keep /STACK.
Config.inc.in: Windows getLibraryPath() (GetModuleHandleExW(FROM_ADDRESS|UNCHANGED_REFCOUNT) + GetModuleFileNameW, UTF-8 out); bundle discovery: on Windows DLL in bin/ -> <prefix>/lib/plumed and <prefix>/include.
Plumed.h: _WIN32 implementation behind __PLUMED_HAS_DLOPEN: LoadLibraryW (UTF-8 -> UTF-16), GetProcAddress, FreeLibrary, error via FormatMessage; RTLD_* flags ignored; keep the "strip Kernel" fallback. Plumed.h is used by all MD codes: guarded, and checked by the existing Linux/macOS wrapper regtests.
install() rules: bin/plumed.exe, bin/plumedKernel.dll, lib/plumedKernel.lib, include/plumed (headers as install.headers), lib/plumed/{vim,json,patches?,src/config} (scripts are bash: ship but expect no use).
CI (windows.yml): build static and shared; with the shared one run the CLI smoke and a small C host using plumed_create_dlopen / PLUMED_KERNEL.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e0e36e2934c6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Wheels opes hdf5 chemfiles

## Description

# Wheels: OPES default-on, HDF5 and chemfiles in all wheels (2026-11-11)

Branch `wheels-opes-hdf5-chemfiles` on master b5ea33b7d (after the platform-features merge), 3 commits, delivered as `wheels-opes-hdf5-chemfiles.bundle` and fetched into the user's checkout; to be pushed by Carlo.

## Commits
1. 0c5beeeab **H5Table: serialize HDF5 calls** (recursive mutex in every public entry point and in Id::reset). Reason: HDF5 is thread safe only if built so (impossible for static libs on Windows: HDF5 CMake FATAL_ERROR), and thread-MPI ranks are threads (rt-h5-mpi: --multi 2 with 4 thread-MPI ranks writes several .h5 at once). Also fixes source builds with a non-threadsafe system HDF5 + thread-MPI. ttsketch (ITensor HDF5) not covered (not in wheels).
2. 78a4f53d8 **opes default-on** (module.type, module.md, README examples now use ves/drr/bias, CHANGES Biases entry, opes added to .ci/windows-regtests dirs: +~32 serial and ~19 thread-MPI tests on Windows). Windows CMake build now compiles opes (default-on set): first MSVC compile of opes happens in CI.
3. 8a3d67785 **HDF5 + chemfiles in wheels**:
   - `python/build_dependencies.py <prefix>`: downloads (sha256-pinned GitHub release archives) zlib 1.3.1, HDF5 1.14.6, chemfiles 0.10.4; CMake static, PIC, Release; Windows /MT (CMAKE_MSVC_RUNTIME_LIBRARY + CMAKE_POLICY_DEFAULT_CMP0091=NEW), CMAKE_POLICY_VERSION_MINIMUM=3.5 (chemfiles min 2.8.12 vs CMake 4); HDF5: no HL/tools/tests/szip/plugins/threadsafe, zlib from the prefix; chemfiles CHFL_SYSTEM_ZLIB=ON (own lzma/bzip2); shared zlib removed after install; licenses in <prefix>/licenses (chemfiles externals + external/README.md); stamp plumed-dependencies.txt. pip-installs cmake if missing. ~8 min on a 4-core Linux box.
   - pyproject: before-all per platform (Linux: /opt/python/cp312-cp312/bin/python; macOS python3; Windows python), PLUMED_DEPENDENCIES=/tmp/plumed-dependencies (C:/plumed-dependencies on Windows) in every environment.
   - build_backend: `_dependencies()`; configure gets --enable-libhdf5 --enable-libchemfiles, CPPFLAGS -I, LIBS = full paths of libchemfiles.a libhdf5.a libz.a (+ -ldl -lm on Linux), LDFLAGS -Wl,--exclude-libs,ALL on Linux; `_drop_dependency_paths` scrubs the prefix from compile_options.sh (otherwise mklib/LOAD link lines point to nonexistent .a), config.txt, *.pc; licenses copied to runtime/licenses; build-info "Libraries linked statically: zlib 1.3.1, hdf5 1.14.6, chemfiles 0.10.4"; cache key includes the stamp. CMake: -DPLUMED_ENABLE_HDF5=ON -DPLUMED_ENABLE_CHEMFILES=ON -DCMAKE_PREFIX_PATH.
   - CMakeLists: options PLUMED_ENABLE_HDF5 / PLUMED_ENABLE_CHEMFILES (find_package(ZLIB) with ZLIB_USE_STATIC_LIBS, find_package(HDF5 NAMES hdf5 CONFIG) -> hdf5-static target (installed in <prefix>/cmake), find_package(chemfiles CONFIG)); --exclude-libs for a shared kernel on Linux; config.txt `has libhdf5/libchemfiles on`. chemfiles' generated exports.h is empty for static builds (no CHFL_STATIC_DEFINE needed).
   - test_bundled_wheel `check_optional_libraries`: config.txt entries (+ module opes on), symbols H5Fopen/chfl_version/deflate not exported (Linux dlsym, Windows WinDLL), HDF5 table written by the kernel with h5py imported first (read back with h5py and `plumed h5convert`), `driver --itraj traj.xyz` with chemfiles (Å -> 0.1/0.2 nm). Dependency check forbids hdf5/chemfiles/libz./zlib.
   - Docs: PLATFORMS.md (wheels row yes for HDF5/chemfiles; Windows CMake options), README python paragraph, python/README.rst, CHANGES.

## Verified in the cloud (Linux)
- Wheel built with configure + static deps: all wheel tests pass (test_bundled_wheel incl. LOAD/mklib after scrubbing, check_optional_libraries with h5py 3.x bundling HDF5 2.0.0 in the same process), test_hdf5 with pandas/h5py.
- regtests with the wheel's plumed: basic/rt-h5-mpi, rt-h5-tables, opes/rt-h5-opes, rt-h5-grids, rt-checkpoint-opes-mpi pass.
- CMake shared kernel on Linux with both options: builds; driver --itraj + .h5 + h5convert work; no exported H5Fopen/chfl_version/deflate; no shared deps.

## Watch in CI
- manylinux2014: cmake availability in before-all (fallback pip), HDF5/chemfiles with gcc 10.
- macOS: MACOSX_DEPLOYMENT_TARGET reaching before-all (script passes CMAKE_OSX_DEPLOYMENT_TARGET when set); kernel exports the static libs' symbols (two-level namespace: not hidden, by choice).
- Windows: zlibstatic.lib found by FindZLIB (ZLIB_USE_STATIC_LIBS, CMake >= 3.24), HDF5 static /MT, chemfiles + PDBTpr/TrajectoryParser/H5Table compiled by MSVC for the first time; opes compiled by MSVC; opes Windows regtests.
- Build time: + ~8-10 min per wheel job (before-all, not cached across runs; actions/cache keyed on build_dependencies.py could be added).
- Possible follow-ups: PLUMED zlib (.gz) support is now free (zlib already linked); HDF5 2.x.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7300c286c2da`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
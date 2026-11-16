# Issue

**Title:** LJ Benchmark: skin retuning

## Description

- **Implemented shared memory parallelization for shot range loop**
- **Formatting**
- **Corrected branching by macro variables**
- **Formatting**
- **For checking scalability**
- **Implemented VerletList by Cabana**
- **Formatting**
- **Corrected macro variable**
- **Corrected the case for without SHAREDMEMORY**
- **Slightly modified the creation of VerletList**
- **Formatting**
- **Fixed bug**
- **Formatting**
- **Modified core/CMakeLists.txt due to warning from cabana and kokkos**
- **Formatting**
- **Added comments**
- **Formatting**
- **Improved computation rate**
- **Formatting**
- **Removed empty cell from calculations**
- **Formatting**
- **Modified rebuild_flag**
- **Small refactoring and Fixed rebuild_flag**
- **Small refactoring**
- **Formatting**
- **Ingnored modernize-use-nullptr in Cabana_Parallel.hpp**
- **Formatting**
- **Corrected CMakeLists.txt**
- **Formatting**
- **Modified ifdef in forces_inline**
- **Fixed compiling Kokkos with clang_tidy**
- **Formatting**
- **Fixed warning: modernize-use-nullptr in Kokkos**
- **Refactoring**
- **Formatting**
- **Deleted some variables**
- **Modified write_particles()**
- **Formatting**
- **Implemented optimizing max_counts for VerletList**
- **Modified creation of VerletList**
- **Formatting**
- **Added load balancing for verlet list**
- **Formatting**
- **Added addNeighborNonAtomic**
- **Added new function in custom_verlet_list.hpp**
- **Formatting**
- **Modifed addNeighbor**
- **Formatting**
- **Modified verlet_list_loop**
- **Experiment for computational rate**
- **Improved computation rate**
- **Formatting**
- **Fixed warning**
- **Formatting**
- **Fixed a bug**
- **Improved computation rate**
- **Formatting**
- **Implemented practical max_counts**
- **Fixed bugs**
- **Formatting**
- **Small refactoring**
- **Formatting**
- **Implemented non-Atomic Add for Verlet List**
- **Formatting and add new header of functor for force calc**
- **Formatting**
- **Small refactoring**
- **Formatting**
- **benchmarks: Add periodic skin retuning and improve timing precision**

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `78a523ccaa36`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** CPU kernel patch for oneAPI compiler performance workaround and code refactoring

## Description

Add macro function to replace inline function (TRANSLATE_PIXEL_2D, TRANSLATE_PIXEL_3D)
Add "USE_SINCOS_TABLE" macro to CPU acceleration code for alternative code path
Add "#pragma forceinline" for project2Dmodel/project3Dmodel functions
Add alignas in CPU kernel function's arrays
Add "#if _OPENMP" to protect "#pragma omp simd" pragmas from old compiler
Add "#pragma omp simd simdlen" for some cases
Some minor code refactoring
Remove "__attribute__((always_inline))" since "inline" is enough
Remove DEBUG_CUDA
Remove unused no_tex2D and no_tex3D in acc/cpu code path
Remove commented old code block in diff2.h

Compilation checked with GNU compiler 7~13, Intel Classic Compiler, and various Intel oneAPI compiler versions.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `adfec821695e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
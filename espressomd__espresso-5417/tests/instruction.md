# Issue

**Title:** Make strategic Utils::Vector operations noexcept

## Description

Kokkos kernels are automatically labeled with the `noexcept` specifier, since exception propagation is not possible on device code and inside OpenMP parallel regions. Add `noexcept` specifiers and `std::floating_point` constraints in key `BoxGeometry` kernels and `Utils::Vector` kernels and constructors. When a throw statement is accidentally introduced in one of those `noexcept` functions, compiler diagnostics will flag the regression. For debug and coverage builds, using a `noexcept` constructor over a regular one also allows elision of the exception handling block in the assembly code, saving one comparison operation per instantiation. The speed-up is minimal, about 1% for a pure LJ simulation with an `-Og` build. Finally, rely on synthetic generation of `Utils::Vector` comparison functions using C++20.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ffe09b24bd76`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Implement utils::sprintf() and use it for format style variables and immediate variable expansion

## Description

**Summary**

This pull request implements an `utils::sprintf()` function that provides printf() style formatting in a std::string context. It follows in spirit the implementation of `fmt::sprintf()` similar to how `utils::print()` follows `fmt::print()` both of which are not available in the std::format support. This new function has no limit in the size of the output string (other than the limits of `std::string` itself) and better argument checking than snprintf()/sprintf().

A future pull request will use this facility to replace the remaining C-library style buffer based formatting from thermo output and dump styles.

**Related Issue(s)**

N/A

**Author(s)**

Axel Kohlmeyer, Temple U

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

The bulk of the implementation was realized using Claude Code (Fable 5) after detailed planning under supervision and with validation of a human.

**Backward Compatibility**

All existing regression tests pass.

**Implementation Notes**

This is implemented as a variadic template `utils::sprintf(const std::string &format, args...)` returning `std::string`, structured like `utils::print()` — the template forwards to an internal C-variadic `utils::varargs_sprintf()`. A `sprintf_arg()` helper sits between them: it passes arithmetic types, pointers, and string literals through unchanged, converts `std::string` arguments to `std::string.c_str()` so they work with %s (a convenience plain snprintf doesn't have), and rejects any other class type with a compile-time static_assert. It cannot catch %d-with-a-double mismatches, which the doc comment says explicitly.

This is realized with vsnprintf() and a 512-byte stack buffer — sized to match thermo's current static char fmtbuf[512], so a future application to thermo output completes in one pass with no allocation. If the result is larger, the conversion repeats once into a heap buffer of exactly the size `vsnprintf` reported (via `va_copy`), so unlike the current call sites the output is never truncated. A conversion error returns an empty string. Deliberately using the C-library semantics and thus preserving the current behavior and runtime %-formats keep working as documented behavior.

**Post Submission Checklist**

- [ ] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [x] Suitable new documentation files and/or updates to the existing docs are included
- [x] The added/updated documentation is integrated and tested with the documentation build system
- [x] The feature has been verified to work with the conventional build system
- [x] The feature has been verified to work with the CMake based build system
- [x] Suitable tests have been added to the unittest tree.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4db85d8bf2c6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
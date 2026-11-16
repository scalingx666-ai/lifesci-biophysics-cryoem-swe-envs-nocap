# Issue

**Title:** scitbx flex as_string: snprintf instead of boost::format

## Description

`flex.double.as_string()` and the integer `as_string()` formatted every element through `boost::format`, which dominates the cost of writing a reflection cif from `iotbx.cif` (the index and data columns are all `as_string()` output). This formats one value with `snprintf` instead.

- The default format string `"%d"` printed a double the way a stream does, six significant digits; that is printf `%.6g`, which is now used directly.
- A format string that is a single printf directive (optional flags, width, precision; `e E f g G` for double, `d i` for integers) is passed to `snprintf` as is, so a caller can pick the digits, e.g. `%.8g`.
- Anything else (literal text around the directive, `%s`, `%x`, `%10d` on a double) still goes through `boost::format`, unchanged.

Speed (macOS, clang; 700k doubles, 500k ints): 383 -> 130 ns per double, 247 -> 47 ns per int. Assembling the cif block for a 3M-reflection dataset goes from 5.0 s to 1.2 s.

Output: byte-identical to the old code over 1.2 M probe values (log-uniform doubles over 60 decades, float32-rounded values, special values, int16/int64 extremes) for the default and for `%.7g %.8g %10.3f %-12.4e %+.3e %g %.0f %12.5G %E`, and for the boost-only forms. Ten cif files from five datasets written before and after are identical.

One behaviour change: `flex.int8.as_string()` used to stream the `signed char` as a character (65 -> "A", -1 could not be decoded into a Python string). It now prints numbers like the other integer types.

Tests: cases added to `scitbx/array_family/boost_python/tst_flex.py` (default on large, small, round and non-finite values, explicit formats, a `%.0f` result longer than the stack buffer, the boost-only forms, int8/int16/int64). The scitbx and iotbx test suites pass.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8a216251df08`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
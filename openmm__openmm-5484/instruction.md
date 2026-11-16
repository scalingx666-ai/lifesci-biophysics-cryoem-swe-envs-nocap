# Issue

**Title:** Optimization: Avoid stringstreams when formatting serialization integers

## Description

Replace stringstream formatting in SerializationNode::setIntProperty() and setLongProperty() with std::to_string(), retaining the existing setStringProperty() delegation.

Tests: MSVC x64 Release benchmarks on the packed-property implementation measured 39–40% less XML serialization time for synthetic mixed Systems with 10,000 and 100,000 particles, using medians of seven runs. Peak memory was essentially unchanged.
All five benchmark workloads produced byte-identical XML under the classic locale, and 49 serialization tests passed against each DLL.

Note: std::to_string() does not apply the global C++ locale’s digit-grouping rules, so output may differ from the existing stream-based formatting under custom locales. From what I can see, OpenMM’s serialization code does not currently guard similar locale-sensitive stream conversions, so this change does not introduce a guard.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `685ded00ddb8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
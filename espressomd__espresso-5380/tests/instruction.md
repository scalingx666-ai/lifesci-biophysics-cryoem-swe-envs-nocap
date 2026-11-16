# Issue

**Title:** Fix RuntimeErrorStream copy ctor

## Description

The user-defined RuntimeErrorStream copy constructor (added because
std::ostringstream is not copyable) initialized m_ec, m_line, m_file and
m_function and copied the buffer content, but omitted m_level from its
member initializer list. Because m_level has no default member initializer,
a copied stream's severity level was left at whatever value already occupied
the storage. The destructor reads m_level and forwards it to
RuntimeErrorCollector::message(), so a copied stream would record a runtime
error/warning with the wrong (or, for genuinely uninitialized storage,
indeterminate) severity. This is latent defensive hygiene today: the
factory returns a prvalue and all call sites are prvalue copy-init under
C++20 guaranteed copy elision, so the copy ctor is currently never invoked.

Fix: add m_level(rhs.m_level) to the copy constructor's init list, matching
the value constructor which already initializes m_level. The copy now
faithfully preserves all members.

Test: src/core/unit_tests/RuntimeErrorStream_test.cpp copy-constructs a
RuntimeErrorStream from an ERROR-level lvalue into a buffer deterministically
pre-filled with the valid WARNING (all-zero) byte pattern, then lets the
copy's destructor record into a RuntimeErrorCollector. On the unfixed copy
ctor the recorded level is WARNING (mismatch); with the fix it is ERROR.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ca2790df2937`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
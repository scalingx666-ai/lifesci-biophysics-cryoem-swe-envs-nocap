# Issue

**Title:** scitbx: fix flex.*_from_byte_str truncation of non-ASCII Unicode input

## Description

- shared_from_byte_str (flex.int_from_byte_str, double_from_byte_str, size_t_from_byte_str, ...) re-encodes a unicode argument to UTF-8 bytes but took its length from len(byte_str), the code-point count; for non-ASCII input that is smaller than the UTF-8 buffer, so the resulting array was silently truncated
- take the length from the byte buffer actually used (PyBytes_GET_SIZE), keeping the pointer and size consistent
- own the re-encoded UTF-8 bytes with handle<>, releasing them; the unicode branch previously leaked the PyUnicode_AsUTF8String reference
- add exercise_from_byte_str_unicode regression test

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ed985e02123b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Normalize periodic box vectors passed as a nested array of Quantities

## Description

The check only looked at the first element, nested Quantity arrays skipped normalization.

---

The section below was generated with AI assistance (Devin) and checked locally before submission.

.

`Topology.setPeriodicBoxVectors` checked only whether the first element was a `Quantity`, so a NumPy object array whose elements are all `Quantity` objects was stored unnormalized. `PDBFile.writeFile` then formatted bare `Quantity` objects as CRYST1 edge lengths and raised `TypeError` from `Quantity.__float__`.

The setter now recognizes a nested sequence of `Quantities` and collapses it into a single `Quantity` wrapping a numeric matrix before the existing validation and storage. Plain numeric sequences and proper `Quantity` inputs are handled exactly as before.

Added `test_BoxVectorsArrayOfQuantities` to `TestPdbFile.py`, which builds the issue's box-vector shape and asserts the CRYST1 edge lengths after a `PDBFile.writeFile` round trip. The new test raises the reported `TypeError` on the unfixed code and passes with the change.

The issue's reproduction script now writes a correct CRYST1 record, and neighboring input forms behave as before (proper `Quantity` input, plain numeric list, invalid reduced-form input still raising `ValueError`). The full Python test suite was not run since a local Python-wrapper build was not available.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ef3f93b9edda`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Write the entry identifier as a data item, not only as the block name

## Description

### The problem

`AbstractCifFileSupplier` wrote the PDB identifier only as the name of the `data_` block, and emitted neither `_entry.id` nor `_struct.entry_id`. Consumers read the identifier as a data item, not from the block header, so it did not survive being written:

- BioJava's own `CifStructureConsumerImpl.consumeStruct` reads `_struct.entry_id`, so `Structure.getPdbId()` came back **null** after a `toText()` / `fromInputStream()` round trip.
- Jmol's mmCIF reader takes `_M.pdbID` from `_entry.id`, so a structure handed over with `openStringInline(structure.toMMCIF())` arrived with no identifier — which in turn breaks anything keyed on the entry, such as `isosurface ... eds`, which needs an entry to fetch the electron density map for.

### The change

Write both categories immediately after entering the block, guarded on the identifier being present (mirroring the existing null check):

```java
blockBuilder.enterEntry().enterId().add(pdbId.getId()).leaveColumn().leaveCategory();
blockBuilder.enterStruct().enterEntryId().add(pdbId.getId()).leaveColumn().leaveCategory();
```

Both are needed, not one: `_entry.id` is the canonical item and the one Jmol reads, while BioJava's own reader only looks at `_struct.entry_id`, so omitting that one would leave BioJava still unable to read back what it just wrote.

`_struct` was not previously written anywhere in this class (only `_struct_keywords`, a separate category), so there is no duplicate-category concern.

### Tests

`CifFileSupplierImplTest` gains two cases — there was previously nothing asserting that an identifier survives `toMMCIF()`:

- `shouldWriteEntryIdAndSurviveRoundTrip` — reads `4hhb.cif.gz`, asserts both items are written, reads the text back and asserts `getPdbId()` still equals `4HHB`.
- `shouldNotWriteEntryIdWhenPdbIdIsAbsent` — a structure with no identifier gains no empty entry categories.

Verified that the round-trip test fails on unpatched `master` (`_entry.id must be written`) and passes with the change; the pre-existing test in that class is unaffected.

### Notes

- Based directly on upstream `master` (`5b29945`); independent of any other open PR.
- Behaviour change is additive — two items appear in written mmCIF files that were absent before, and only when the structure carries an identifier.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d359494c5b6f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
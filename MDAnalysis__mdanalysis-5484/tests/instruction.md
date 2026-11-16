# Issue

**Title:** ENH: Speedup mmCIF parsing by using `gemmi.FlatStructure`

## Description

In the original #4712 the conversion from `gemmi.Structure` to `mda.Universe` was several nested loops as the `gemmi.FlatStructure` wasn't released / complete enough to use for parsing. 

I didn't try and update because I wanted to get _some version_ merged before 2.11. As we haven't released the 2.11 yet, this bumps `gemmi>=0.7.5` from `0.7.3` which includes the proper flat structure required, and updates the topology and coordinate parsers to use it, with some massive performance improvements.

Gemmi supports returning columns like `Atom Name` as a integer lookup into a string array, which is what MDA uses internally. For additional speedup I've also added a helper to construct directly from that which added some extra speed improvements.

Changes made:
- `MMCIFReader`: coordinates now come from `FlatStructure.pos` instead of a nested list comprehension.
- `MMCIFParser`: the per-atom loop is now column reads. Each string column (names, elements, chain IDs, etc.) is split into a per-atom index plus its distinct strings in one `np.unique` over an integer view of gemmi's fixed-width bytes. Residues and segments are then grouped on those indices instead of on strings.
- `_StringInternerMixin._from_lookup` (private, in `core/topologyattrs.py`): builds a string attribute directly from an index array plus its distinct values. This skips the per-value loop in `__init__`.
- `FlatStructure` includes every model, so both paths drop all models after the first. This happens after the existing multi-model warning.
- Minimum gemmi version goes from `0.7.3` to `0.7.5`
- Added additional test file 5PTI.cif.gz for testing with unknown elements

### Performance

| File | Atoms | Topology parse | `mda.Universe(...)` |
|---|---|---|---|
| 1BD2 | 6,378 | 42.9 → 2.0 ms | 87.1 → 12.9 ms |
| 2K9Q (20 models) | 2,522 | 10.5 → 1.1 ms | 85.4 → 68.3 ms |
| 4V6X | 237,685 | 1188 → 70 ms | 2405 → 498 ms |

## LLM / AI generated code disclosure
<!-- Please update this disclosure to reflect if you did or did not use LLMs / AI to generate code -->
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: no

I had Claude double check everything and run the benchmarks but the implementation was mine.

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [x] Issue raised/referenced?
 - [x] Tests updated/added?
 - [x] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)
 - [x] I have read and understand the current [AI Policy](https://github.com/MDAnalysis/mdanalysis/blob/develop/AI_POLICY.md)
 - [x] LLM/AI disclosure was updated.

## Developers Certificate of Origin
<!-- 
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4f6d54a9e9b7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
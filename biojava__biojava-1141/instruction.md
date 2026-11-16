# Issue

**Title:** Support the new ECOD distribution format

## Description

.

ECOD redesigned its distribution at v294.1. Neither the version comment nor the column layout is recognised any more, so `EcodInstallationTest.testVersion` fails and — less visibly — anyone calling `EcodFactory.getEcodDatabase()` today gets an empty domain list with no exception thrown.

## The version comment

| Release | Header line |
|---|---|
| up to develop292 | `#ECOD version develop291` |
| v294.1 onwards | `# Version: v295` |

`parsedVersion` stayed null and `getVersion()` fell back to the string that was *requested*, which for `latest` is the literal `"latest"` — the value the test asserts against. One pattern now matches both forms, so the historical releases keep working; a regex that only accepted the new form would silently break every pinned `develop*` version.

## The columns

| Format | Columns | Column header row |
|---|---|---|
| v1.4 / v1.5 (develop124, develop204) | 15 | `#uid …` |
| v1.6 (develop291) | 16 — `f_id` renamed `t_id`, `unp_acc` inserted at 9 | `#uid …` |
| v294.1 | 23 | `uid …` — no longer commented out |
| v295 | 25 — adds `ligand_comp_ids`, `ligand_pdbnum` | `uid …` |

Rather than add a fourth positional special case, files that declare a column header — every release since develop101 — are now read **by column name**. That covers all four layouts at once and will not need revisiting the next time a column moves. Files older than develop101 have no header and keep the existing positional path, unchanged.

Everything else that moved with the columns:

- `manual_rep` holds `True`/`False` rather than `MANUAL_REP`/`AUTO_NONREP`;
- `assembly_id` is declared but empty on all 2,945,500 rows, meaning what `NOT_DOMAIN_ASSEMBLY` used to mean;
- the ligand list moved to `ligand_comp_ids`;
- `ligand_pdbnum`, the last column, is empty on 2,399,569 rows, so `split("\t")` was dropping it and making those rows look a column short. Trailing empty fields are now kept;
- `f_name` is empty rather than `F_UNCLASSIFIED` for unclassified domains — deliberately left as it is, see below;
- **1,573,776 rows (53%) describe domains found in AlphaFold models**, with ids like `P44140_F1_nD2` and an empty `pdb` column. `EcodDomain` is keyed by `PdbId` and cannot represent them, so they are counted and reported at INFO rather than each logged as an error.

Two failure modes that hid this are also closed: parsing *nothing at all* is now logged as an error instead of quietly returning an empty list, and the per-line `NumberFormatException` warning is capped like the three warnings beside it, rather than printing a stack trace per row.

## `getVersion()` no longer parses the whole file

It reads the header. Parsing the current release in full to answer "which version?" costs **1,186 MB of heap and 9.8 s** — and the test JVM is given `-Xmx1500M` by the root pom. The download is unchanged; only the parse is skipped. This is a small behaviour change, in that `getVersion()` no longer has the side effect of loading every domain, which the 7.3.0 target allows.

## Measured

| File | domains before | domains after |
|---|---|---|
| `ecod.develop124.domains.txt` | 468,680 | 468,680 |
| `ecod.develop204.domains.txt` | 592,427 | 592,427 |
| `ecod.develop291.domains.txt` | **0** | 1,083,021 |
| `ecod.latest.domains.txt` (v295) | **0** | 1,371,723 + 1,573,776 model rows reported |

The first two are the counts `EcodInstallationTest` already asserts, so the old path is provably untouched.

## Tests

`EcodParserTest` is new and entirely offline. `EcodParser` already accepts a `Reader`, so fixtures taken verbatim from the real files pin all five layouts, both version comment forms, a row ending in an empty column, an AlphaFold-derived row and a malformed row. 15 tests, no network, well under a second:

```
mvn test -pl biojava-structure -Dtest=EcodParserTest
```

That is the part I would most like kept. The distribution is 657 MB, so the only test that ever touched the current release was `testVersion`, asserting on one string; everything else pins `develop204`. That is exactly why a wholesale format change surfaced as a single one-line assertion failure, months after it happened.

`testVersion` now also parses the first few thousand lines of the local file — enough to notice a column change without building three million domains.

```
mvn test -pl biojava-integrationtest -Dtest=EcodInstallationTest
```

9 tests, 1 skipped (`testAllVersions`, already `@Ignore`d for being slow), green in 124 s.

## Note on CI

Branched off `master`, so its `PR Build` will still show the CATH failure until #1133 merges. That is #1138, not something new here.

## Three judgement calls I would rather not make alone

@sbliven — you wrote this parser and know the format history far better than I do. If you have the time, I would value your eyes on it, particularly on:

1. **`unp_acc` is read and discarded.** develop291 carries a UniProt accession that `EcodDomain` has nowhere to put. Adding a field means touching `equals`/`hashCode`/`toString`/`Comparable`, so I left it out — but it is real data we are dropping.
2. **An empty `f_name` is left empty rather than translated to `F_UNCLASSIFIED`.** They used to be equivalent; now `f_id` classifies to a fourth level (`1.1.1.3`) while the name is blank, so I do not think they are the same thing any more. If you disagree it is a one-line change.
3. **An empty `assembly_id` is mapped onto the old `NOT_DOMAIN_ASSEMBLY` semantics** (`assemblyId = uid`). It is empty on every row of v294+, so assembly information is simply gone from the distribution; this keeps `getAssemblyId()` returning what callers expect rather than null.

Also deliberately out of scope, and worth a follow-up issue rather than growing this PR: `valid_structure`, `ligand_binding`, `ligand_pdbnum`, `range_count` and the five `*_manual` flags are new columns with no home in `EcodDomain`, and the AlphaFold half of the distribution needs a model that is not keyed by `PdbId`.

## Unrelated but useful

ECOD now publishes `ecod.latest.md5` in coreutils format alongside the distribution. That is a real `hashURL` for `FileDownloadUtils.createValidationFiles(url, file, hashURL, Hash.MD5)` — the path #1133 repairs and which, before it, could never write a hash file at all. Not wired up here; noting it so it is not lost.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4ab9f4afd59d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
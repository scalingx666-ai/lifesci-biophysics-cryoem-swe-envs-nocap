# Issue

**Title:** Parallelizes `MDAnalysis.analysis.InterRDF` and `MDAnalysis.analysis.InterRDF_s`

## Description

Changes made in this Pull Request:
 - Parallized both `rdf.InterRDF` and `rdf.InterRDF_s` 

TLDR: of the comments below: Initially I thought `rdf` isnt parallizable but turns out both classes in `rdf `can be parallelized.

PR Checklist
------------
 - [x] Tests?
 - [x] Docs?
 - [x] CHANGELOG updated?
 - [x] Issue raised/referenced?

## Developers certificate of origin
- [x] I certify that this contribution is covered by the LGPLv2.1+ license as defined in our [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE) and adheres to the [**Developer Certificate of Origin**](https://developercertificate.org/).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--4884.org.readthedocs.build/en/4884/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `519ac5682528`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
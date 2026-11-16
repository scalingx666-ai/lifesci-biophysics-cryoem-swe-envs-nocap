# Issue

**Title:** Add readmultimmcif, writemultimmcif functions

## Description

# Add readmultimmcif, writemultimmcif functions

An mmCIF file can have multiple data_ blocks, thereby containing multiple `MMCIFDict`s. An example of such a file is the Chemical Components Dictionary from the Protein Data Bank. The added functions allow reading and writing such files.

I added new functions in order to not have to change existing functions and the API. The existing parsing infrastructure was used as much as possible.

## Example
Using the [Chemical Components Dictionary](https://www.wwpdb.org/data/ccd) mmCIF file:
```julia
julia> ccd = readmultimmcif("components.cif.gz"; gzip=true)

julia> ccd["2WA"]
mmCIF dictionary with 64 fields

julia> ccd["HNG"]
mmCIF dictionary with 69 fields
```


## Types of changes

This PR implements the following changes:
_(Please tick any or all of the following that are applicable)_

* [x] :sparkles: New feature (A non-breaking change which adds functionality).
* [ ] :bug: Bug fix (A non-breaking change, which fixes an issue).
* [ ] :boom: Breaking change (fix or feature that would cause existing functionality to change).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cbdb7df2fd33`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
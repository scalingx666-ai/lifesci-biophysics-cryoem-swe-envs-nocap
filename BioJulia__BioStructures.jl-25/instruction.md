# Issue

**Title:** Add MMCIF reading and writing with gzip compression

## Description

# Add MMCIF reading and writing with gzip compression 

This adds a gzip keyword in the same manner as the MMTF functions have.

Sorry that the patch for the tests is rather noisy and hard to compare, but that was the easiest way to test many things without adding a lot of extra code.  Let me know if you want it changed in any way.

I also didn't add anything to the downloadpdb... functions.  Not sure what the best way do that would be, perhaps a decompress=true keyword.  I personally don't use those functions at the moment (using the PDB rsync).

I have not checked any influence on performance, hoping that there is none.

## Types of changes

This PR implements the following changes:
_(Please tick any or all of the following that are applicable)_

* [x] :sparkles: New feature (A non-breaking change which adds functionality).
* [ ] :bug: Bug fix (A non-breaking change, which fixes an issue).
* [ ] :boom: Breaking change (fix or feature that would cause existing functionality to change).

## :clipboard: Additional detail

- If you have implemented new features or behaviour
  - **Provide a description of the addition** in as many details as possible.
See above

  - **Provide justification of the addition**.
Makes it easier to work with gzip-compressed mmcif files, as in the mmtf case.

  - **Provide a runnable example of use of your addition**.

At the shell:
```sh
    $ wget https://files.rcsb.org/download/1AKI.cif.gz
```

In Julia:
```julia
using BioStructures
cif = MMCIFDict("1AKI.cif.gz"; gzip=true)
```

  - **Does your change alter APIs or existing exposed methods/types?**
This adds a new keyword argument to mmcif functions, which (i believe) shouldn't affect existing usage.

## :ballot_box_with_check: Checklist

Note: the julia style guide link and the documentation style guide links don't work.

- [ ] :art: The changes implemented is consistent with the [julia style guide](https://docs.julialang.org/en/stable/manual/style-guide/).
- [x] :blue_book: I have updated and added relevant docstrings, in a manner consistent with the [documentation styleguide](https://docs.julialang.org/en/stable/manual/documentation/).
- [x] :blue_book: I have added or updated relevant user and developer manuals/documentation in `docs/src/`.
- [x] :ok: There are unit tests that cover the code changes I have made.
- [x] :ok: The unit tests cover my code changes AND they pass.
- [ ] :pencil: I have added an entry to the `[UNRELEASED]` section of the manually curated `CHANGELOG.md` file for this repository.
- [x] :ok: All changes should be compatible with the latest stable version of Julia.
- [x] :thought_balloon: I have commented liberally for any complex pieces of internal code.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `68c891a62371`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
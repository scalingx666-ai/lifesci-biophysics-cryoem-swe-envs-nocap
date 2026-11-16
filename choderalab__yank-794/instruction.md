# Issue

**Title:** Arbitrary Molecular Regions in YAML

## Description

This PR adds arbitrary region identification to the YAML documentation in continuation of the abilities added by #793.

Regions can be defined for any `molecules` entry and as many as you would like can be defined. These regions are validated to accept ints, strings, or lists of ints, although only strings is non-ambiguous and even improvements could be made upon that as well, but the framework is here.

~Although functionally complete, this is a Work In Progress for the following reasons:~ *edit*: no longer a WIP after additions.
- [x] Error message when `check_regions_clash` raises a `YamlParseError` is absolute garbage and falls back to an incorrect error message, this is a larger problem with YamlParseError in general though 
- [x] Documentation on the `docs` pages have not been added yet. 

The one functional improvement we should make is to have each molecule's region's only apply to the subset of atoms that molecule makes up in the final Topography. I think this would require a small change to the `add_region` call of Topography + some logic in `_build_experiment`, but may not be too hard. Debate on whether we make that change here or in another PR after we merge this one since this feature set is suppose to be a series of smaller PR chunks, each adding new infrastructure an leading up to the full feature. Thoughts?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `247d7d1ea2ad`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
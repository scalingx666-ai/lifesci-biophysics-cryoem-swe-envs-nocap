# Issue

**Title:** fix issue with logic, add debug statements

## Description

## Description
There was a bug in the logic where if we saved a file with a `.gz` extension, we would save the file compressed, then overwrite the file with an uncompressed version.

## Motivation and context

Since this commit: 1145a7405f4beb0de10ab80aa80f6e01aa1a8794 we have not actually been compressing our xml files.

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

Tested locally. I could be convinced to write a test, but with the refactoring coming up, I think will prefer to use a different method for this. 

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
 objects serialized with utils.data.serialize now will be compressed with bzip2 or gzip depending on file name (.gz and .bz2, respectively)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2ff6ecceb8d5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
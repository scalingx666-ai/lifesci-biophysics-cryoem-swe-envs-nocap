# Issue

**Title:** Feat/enable cloud io

## Description

## Description

<!-- Describe your changes in detail. -->

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Added upport for reading input files (ex yaml, sdf, pdbs) from AWS, GCP, and Azure. See the documentation for [cloudpathlib](https://cloudpathlib.drivendata.org/stable/authentication/) for how to setup authentication. Currently only reading the yaml from S3 is unit tested (ie `perses-cli --yaml s3://perses-testing/template_s3.yaml`), but other cloud providers and input files should work. Please report any issues on our issue tracker! 

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `87bc1d00c6c0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
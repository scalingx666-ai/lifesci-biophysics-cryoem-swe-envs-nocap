# Issue

**Title:** only split after first colon when doing an override

## Description

## Description

<!-- Describe your changes in detail. -->

Allows users to specify an override like `--override  foo:bar:baz` to be split into `foo` and `bar:baz` instead of `foo` `bar` `baz` which would break our parser. Common use case is to have a url as a key, ie `some_path:http://s3.aws.com`

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->

Tested locally.

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Fixes bug where if a `:` was in a key, we could not override the arguement in our perses-cli. 
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c4368a6f666d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
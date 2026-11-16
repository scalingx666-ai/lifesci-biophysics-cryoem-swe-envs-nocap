# Issue

**Title:** use a local context cache instead of a global one

## Description

## Description

Instead of using the global context cache (which is the default) create a instance of the context cache and use that for all the mcmc move types. I also updated an example code snippet as well.

## Motivation and context

See this issue 


## How has this been tested?

Tested on CI, issue linked above also shows that this fix works for large systems.

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Use an instance of ContextCache instead of the default global instance.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ba3e04ea5dcc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
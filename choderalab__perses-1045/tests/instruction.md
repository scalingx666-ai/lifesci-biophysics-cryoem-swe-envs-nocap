# Issue

**Title:** `RESTCapableHybridTopologyFactory` working with small molecules

## Description

## Description

the `lambda_protocol` option for the `HybridRepexSampler` object was being specified, forcing `LambdaProtocol()` (which can only be used with `HybridTopologyFactory`, not `RESTCapableHybridTopologyFactory`) to be used. `lambda_protocol` is now left undefined, so that the `HybridRepexSampler` will automatically detect and choose the appropriate lambda protocol according to the HTF.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

Tested locally with partial tyk2 benchmark simulation

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Bug fix when trying to use the new `RESTCapableHybridTopologyFactory` in the small molecule pipeline -- fixed by not specifying a protocol, hence allowing the `HybridCompatibilityMixin` object to automatically handle it.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `af66f3161bec`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
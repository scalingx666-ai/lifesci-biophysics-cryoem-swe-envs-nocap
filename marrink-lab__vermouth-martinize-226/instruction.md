# Issue

**Title:** Add the notion of domains in the elastic network

## Description

* Add the notion of elastic network domains. Bonds are only added within a domain.
* Add the `-eunit` option to the CLI to specify what the domains are. The valid options are 'all', 'chain', and 'molecule'.

In the future, `-eunit` could accept an index file or a domElnedyn input file. Having that working would mostly mean writing a function that can be use as argument for the `domain_criterion` argument of the processor.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `18e1b3645590`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
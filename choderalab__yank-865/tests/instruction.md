# Issue

**Title:** Add option to pipeline to use pdbfixer to make protein mutations

## Description

Addresses #69 

I'm having a bit of trouble with the tests, but this should otherwise be mostly complete.

For single mutations, simply use the familiar one-letter-code mutation:
```yaml
   molecules:
     Abl:
       make_mutations:
         chain: A
         mutations: T315I
```
For double mutations, use the '/' separator:
```yaml
   molecules:
     EGFR:
       make_mutations:
         mutations: L858R/T790M
```
when no `chain` is specified, it should default to `None`.

If needed, we could add the capability to support mutations in chains with different chain ids. Currently, this is not supported.

I believe you should be able to automatically use `!$Combinatorial` syntax with this as well to provide a list of mutations, which is our intended use case. (Let us know if that isn't the case!)

I don't like the idea of ingesting the one-letter codes and their maps, but they can later be refactored into something else (or, ideally, the functionality to support more flexible syntax put into `pdbfixer` before it is released).

Reviews/feedback welcomed when able!

cc: @steven-albanese

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c34fa898e415`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Improve mvn release process

## Description

Changes to pom needed to perform a clean release to maven. I've tested it to produce a test 6.0.0-alpha1 and it worked fine. With this, releasing must now be done using the `release` profile, i.e. `mvn -Prelease release:prepare` and `mvn -Prelease release:perform`

Did some other pom cleanups too.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `53c2dc0c3807`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
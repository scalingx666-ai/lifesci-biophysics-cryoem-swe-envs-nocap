# Issue

**Title:** Make biojava work with java 9, 10, 11

## Description

Solves #804. Thanks @sbliven for most of the work.

This does:
- Keeps Java 8 compliance (compiled byte code is 8)
- Makes biojava work with java 9, 10 or 11 JREs

At some later point we can decide to move to next version of compliance (9, 10 or 11). That only requires changing the properties `jdk.version` and `maven.enforcer.jdk-version` in main pom.xml.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f7a2a1654f2a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Junit5 upgrade

## Description

This PR demonstrates using JUnit 5(Jupiter) in BioJava

Work done:

- added dependency declarations to <dependencyManagement> section of top-level pom
- added junit5 dependency to biojava-core
- refactored 2 unit tests - ABITracerTest and TestAmbiguityCompoundSet to  use junit5 and demo a few of the junit5 features such as parameterised tests and  displayName.
![Screenshot 2021-07-04 at 10 57 49](https://user-images.githubusercontent.com/22676399/124380797-b3955900-dcb6-11eb-8993-0cf3c9b9b7b9.png)

- ran entire test suite, all passed on my dev machine
- setup follows guidelines here: https://junit.org/junit5/docs/current/user-guide/#migrating-from-junit4
- So far I've just tested in biojava-core. 
- Like the junit3/4 migration, idea is to enable possibility to write new tests using JU5 and update existing tests over time at own pace.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c0049f50b5e4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
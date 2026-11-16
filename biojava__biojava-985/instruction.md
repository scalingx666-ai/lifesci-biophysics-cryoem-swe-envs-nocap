# Issue

**Title:** Fix flaky in TestQuatSymmetryDetectorExamples

## Description

The test `org.biojava.nbio.structure.test.symmetry.TestQuatSymmetryDetectorExamples#testLocal` in `biojava-integrationtest` is flaky. It sometimes fail when changing the random seeds.
The root cause of the problem is `calcLocalSymmetries` uses a `Set` to store `usedClusterIds`. This will result an unguaranteed order in returned `Stoichiometry`.
This fix is to use an ordered `TreeSet` instead of `Set` to reduce the flakiness in the test.

To reproduce the failing test, use the following command. It basically run the same test multiple times with different random seeds.
`mvn -pl biojava-integrationtest edu.illinois:nondex-maven-plugin:1.1.2:nondex -Dtest=org.biojava.nbio.structure.test.symmetry.TestQuatSymmetryDetectorExamples#testLocal`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `32e89927bcc4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
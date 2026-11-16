# Issue

**Title:** Change JAXB implementation for better java version compatibility

## Description

There was an issue with running CeSymm on java 15+ (rcsb/symmetry#104). I suspect that this issue effected all projects that include biojava with the maven-shade-plugin. I was able to trace the problem back to the old com.sun.xml.bind implementations. Switching these to the glassfish JAXB implementation fixed the problem.

- Downgrade jgrapht to maintain java 8 support
- Switch JAXB to glassfish implementation for better java 15+ support

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `baefba4b3e6e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Fix compilation bug in ZipChemCompProvider

## Description

With some JDKs (openjdk-11) there is an (undocumented?)
`java.nio.file.FileSystems.newFileSystem(Path,Map<String,?>)`
method. This patch disambiguates which override to use for null.

Without it I was seeing the following compilation error:
```
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:3.8.0:compile (default-compile) on project biojava-structure: Compilation failure: Compilation failure:
[ERROR] /Users/bliven_s/ownCloud/dev/biojava_ws/biojava/biojava-structure/src/main/java/org/biojava/nbio/structure/io/mmcif/ZipChemCompProvider.java:[239,49] reference to newFileSystem is ambiguous
[ERROR]   both method newFileSystem(java.nio.file.Path,java.lang.ClassLoader) in java.nio.file.FileSystems and method newFileSystem(java.nio.file.Path,java.util.Map<java.lang.String,?>) in java.nio.file.FileSystems match
[ERROR] /Users/bliven_s/ownCloud/dev/biojava_ws/biojava/biojava-structure/src/main/java/org/biojava/nbio/structure/io/mmcif/ZipChemCompProvider.java:[296,52] reference to newFileSystem is ambiguous
[ERROR]   both method newFileSystem(java.nio.file.Path,java.lang.ClassLoader) in java.nio.file.FileSystems and method newFileSystem(java.nio.file.Path,java.util.Map<java.lang.String,?>) in java.nio.file.FileSystems match
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3a06c1eb2f5b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
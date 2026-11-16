# Issue

**Title:** Tests for core util package

## Description

Added test-classes for several util classes. Mostly routine, a few comments. Planned to add tests for all remaining util methods but that would be a monster PR so breaking into 2. 
- added Javadocs I thought would be useful after discovering behaviour through testing
- In a few cases added some obvious error handling  but made minimal changes to source code
- FileDownloadUtils is tricky to test some of the download methods, without doing some refactoring to mock/stub the actual network call and be able to configure timeout to not be hardcoded to 60s
- SingleLinkageClustererTest is quite basic. I followed the Wikipedia example entry and the results from the code are quite similar but not identical to the worked example. Not sure if this is important or relevant in the context in which it's used but can provide more details if needed.
- I can't think of any way to 'fake' memory usage for SoftHashMapTest, so there is a @Disabled test which can be run after setting -Xmx to a low value e.g. 5M. And the test is  slightly non-deterministic as well.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `600763eebe45`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
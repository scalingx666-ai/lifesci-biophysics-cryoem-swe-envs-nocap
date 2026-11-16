# Issue

**Title:** API for querying devices

## Description

Implements #5157.  This adds a `getDevices()` method to Platform.  It returns a list of maps, one for each available device.  They contain the values for platform-specific properties.  You can pass them directly as the `properties` argument when creating a Context to select the corresponding device.

There's an optional `filters` argument which takes property values.  It will only return devices that are compatible with those values.  For example, you might pass `{'Precision':'mixed'}` to only get devices compatible with mixed precision, or `{'OpenCLPlatformIndex':'1'}` to specify a particular OpenCL implementation.

cc @chrisiacovella

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `421eb42a6948`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
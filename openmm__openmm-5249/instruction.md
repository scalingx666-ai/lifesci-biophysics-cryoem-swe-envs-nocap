# Issue

**Title:** Reduced memory used by SerializationNode

## Description

When loading or saving an object (system, state, integrator) serialized to XML, it needs to create a tree of SerializationNode objects describing the complete content of the file.  Those objects take up several times more memory than the size of the file.  They only exist transiently during serialization and deserialization, so usually it isn't a problem.  But for very large systems with millions of particles, the required memory can sometimes be a problem.  .

This PR changes how a SerializationNode stores its data internally to reduce the memory use.  I tested on a serialized system and found the memory required for all the SerializationNodes was reduced to only about 38% of what it was before.  That was compiling with clang on a Mac.  MSVC has a less efficient implementation of strings, so on Windows the reduction is likely to be even larger.

I was concerned this might hurt speed, since looking up the value of a property is now an O(n) operation in the number of properties for a node, but I don't see any significant change.  The time to execute `deserialize()` for the system I tested only increased by about 3%.

The changes are mostly transparent and don't affect invoking code.  There are a few minor changes to the API to be aware of.  You can only set the value of a property once.  Previously, subsequent calls would overwrite the value, but now they don't work correctly.  There's no good reason to do this, and the only place I ever did it was in a test case.  Also, `getProperties()` and `getStringProperty()` now return new objects instead of references to internal objects.  Usually this makes no difference, but if you expected repeated calls to `getProperties()` to return the same map each time, it won't anymore.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e4728a2160d2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
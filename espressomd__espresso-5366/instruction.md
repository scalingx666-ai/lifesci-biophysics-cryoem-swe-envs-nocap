# Issue

**Title:** core/io: fix H5MD charge/force sharing the same enum bit

## Description

H5MD_OUT_CHARGE and H5MD_OUT_FORCE both equalled 16u in the
H5MDOutputFields enum, so every `fields & H5MD_OUT_CHARGE` test also
fired for force and vice versa. Requesting fields=['particle.charge']
silently created and wrote the force dataset too (and symmetrically),
and 'charge only' / 'force only' selections could not be expressed.

Give charge its own distinct bit (64u, previously unused; the enum
jumped 32u MASS -> 128u BONDS). H5MD_OUT_ALL already covers bit 64, so
the default 'all' selection and existing 'all' trajectories are
unaffected. Charge and force are independent particle properties that
must both be expressible, so this HANDLEs the case rather than
preventing one of the two fields.

Add a regression test (test_selective_fields in testsuite/python/
h5md.py) asserting that requesting a single field creates only that
field's dataset.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cf44d3180db2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
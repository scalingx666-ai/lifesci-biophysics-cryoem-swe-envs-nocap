# Issue

**Title:** Remove redundant SQLite connections

## Description

As per #685, the `ASEAtomsData` class might create many (thousands) database connections that are only very briefly used. This PR replaces these with the already-available open database connection created upon initialization of the class instance.

Two `with connect():` context managers are kept:
- [within `_check_db()`](https://github.com/lkkmpn/schnetpack/blob/d08f9d2bb9840bd2f96aef1d6e49befc14e62504/src/schnetpack/data/atoms.py#L286-L287), which is called just before `self.conn` is created;
- [within `create()`](https://github.com/lkkmpn/schnetpack/blob/d08f9d2bb9840bd2f96aef1d6e49befc14e62504/src/schnetpack/data/atoms.py#L438-L443), which is a static method run outside of the context of an initialized database.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `527c1d879fb5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
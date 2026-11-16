# Issue

**Title:** Add timestamp and ligands name to output parsed yaml file

## Description

## Description

Adds timestamp and ligands names (old and new) metadata to the generated perses yaml file.

Wrote a new function that encapsulates the generation of the parsed YAML file and adds the mentioned metadata.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?
Created automated unit test for yaml generation in `test_relative_setup.py`. Also tested locally with running the `examples/protein-ligand/cli` example.

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Perses output yaml file now adds timestamp and ligands names information (for old and new ligands).
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7148b229805f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
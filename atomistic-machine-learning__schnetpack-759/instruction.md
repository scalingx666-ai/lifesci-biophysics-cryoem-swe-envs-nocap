# Issue

**Title:** Use caching in uuid resolver

## Description

Currently, calling the uuid resolver will always result in a new uuid. For example, if we have the following config
``` yaml
run:
  path: /home/models
  id: ${uuid:1}

data:
  path: ${run.path}/${run.id}/data
```
the model will be stored in /home/models/first-uuid and the data will be stored in /home/models/another-uuid/data since OmegaConf will call the resolver twice and generate another uuid for `data.path`. By setting `use_cache=True` in the definition of the resolver, the resolver is not evaluated again when referencing `${run.id}` somewhere in the config. In the given example, the data would now be stored in /home/models/first-uuid/data.

The caching in OmegaConf uses the given arguments, i.e. with `use_cache=True` the resolver will always return the same value as long as `${uuid:1}` is used (either directly or indirectly by reference as in the example above). Accordingly, if someone would like to generate two uuids in the config, this would still be possible by using different arguments (e.g. `${uuid:2}`).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `77f735ed0d38`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
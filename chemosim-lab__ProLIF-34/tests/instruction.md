# Issue

**Title:** Update Read the Docs configuration (automatic)

## Description

Howdy! :wave:

I am @readthedocs-assistant and I am sending you this pull request to upgrade the configuration of your Read the Docs project.
Your project will continue working whether or not you merge it, but I recommended you take it into consideration.

Also, in case you haven't done it already, remember that you can enable the pull request builds for your project to see the effect of these changes. To do it, [follow the instructions](https://docs.readthedocs.io/en/stable/pull-requests.html), close this pull request, and open it again.

_*Note*: This tool is in beta phase. Don't hesitate to ping @astrojuanlu and/or @humitos if you spot any problems._

The following migrators were applied:

- Migrate to `build.tools` configuration.

This uses the new base Docker image based on Ubuntu 20.04 introduced in October 2021
and picks an appropriate Python version for your project
(read [our blog post](https://blog.readthedocs.com/new-build-specification/)
for details).
Notice that now you can specify the Node.js, Rust, and Go versions as well.

*Note:* Some system dependencies are not preinstalled anymore,
so this might require manually adding them to `build.apt_packages`
(see [our
documentation](https://docs.readthedocs.io/en/stable/config-file/v2.html#build-apt-packages>)).

- Migrate to Mamba as a drop-in replacement for Conda.

Your project requested using Mamba instead of Conda for performance reasons.
Now this is included in your configuration
and you can change it without our intervention.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `106028c72d88`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
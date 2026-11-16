# Issue

**Title:** Update dependency prefix-dev/pixi to v0.81.0

## Description

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [prefix-dev/pixi](https://redirect.github.com/prefix-dev/pixi) | uses-with | minor | `v0.73.0` → `v0.81.0` |

---

### Release Notes

<details>
<summary>prefix-dev/pixi</summary>

### [`v0.81.0`](https://redirect.github.com/prefix-dev/pixi/releases/tag/v0.81.0)

[Compare Source](https://redirect.github.com/prefix-dev/pixi/compare/v0.80.0...v0.81.0)

##### \[0.81.0] - 2026-09-15

##### ✨ Highlights

`pixi run --script` can now run `conda-script` files directly from HTTP or HTTPS URLs, including GitHub Gists. Pixi downloads the script, reuses its cached environment, and runs it without requiring a local file.

##### Added

- Add remote url support to conda-script by [@&#8203;ruben-arts](https://redirect.github.com/ruben-arts) in
- Add PrefixPlatformMismatchError for source build prefixes by [@&#8203;hunger](https://redirect.github.com/hunger) in
- Show the build platform of nested derived environments by [@&#8203;hunger](https://redirect.github.com/hunger) in

##### Changed

- Track the platform of a derived environment explicitly by [@&#8203;hunger](https://redirect.github.com/hunger) in
- Keep nested derived environments on the build platform by [@&#8203;hunger](https://redirect.github.com/hunger) in

##### Documentation

- Explain how source build-dependencies are built when cross-compiling by [@&#8203;hunger](https://redirect.github.com/hunger) in

##### Fixed

- Use only tombi for TOML formatting by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in

##### Refactor

- Return CLI exit codes from main by [@&#8203;baszalmstra](https://redirect.github.com/baszalmstra) in

##### New Contributors

- [@&#8203;prefix-dev-pixi-upgrades](https://redirect.github.com/prefix-dev-pixi-upgrades)\[bot] made their first contribution in

### [`v0.80.0`](https://redirect.github.com/prefix-dev/pixi/releases/tag/v0.80.0)

[Compare Source](https://redirect.github.com/prefix-dev/pixi/compare/v0.79.0...v0.80.0)

##### \[0.80.0] - 2026-09-07

##### ✨ Highlights

Pixi now has experimental support for `conda-script`. Conda scripts let you write scripts in any language, with dependencies and instructions for running them included in the same file.
Pixi solves the environment, caches it, and runs the script without requiring a separate workspace.

For example, an R script can now be self-contained:

```r

# /// conda-script
# channels = ["https://prefix.dev/conda-forge"]

# entrypoint = "Rscript ${SCRIPT}"
#

# [dependencies]
# r-base = "*"

# r-jsonlite = "*"
# /// end-conda-script
library(jsonlite)
writeLines(toJSON(list(hello = "pixi"), auto_unbox = TRUE))
```

Or compile and run a C++ file with Conda-provided dependencies:

```cpp
// /// conda-script
// channels = ["https://prefix.dev/conda-forge"]
// entrypoint = "g++ -o ${CACHE}/hello ${SCRIPT} -lfmt && ${CACHE}/hello"
//
// [dependencies]
// gxx = "*"
// fmt = "*"
// /// end-conda-script

#include <fmt/core.h>

int main() {
    fmt::print("Hello from pixi!\n");
}
```

First enable the experimental configuration with:

```shell
pixi config set experimental.conda-script true --global
```

Then, run them with:

```shell
pixi run --script hello.R
pixi run --script hello.cpp
```

You can also create and manage these scripts with `pixi init --script`, `pixi add --script`, `pixi remove --script`, `pixi lock --script`, and the other script-aware commands.

##### Added

- Add experimental `conda-script` support by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in , , , , , and . This includes the metadata model, cross-platform entrypoint shell, `pixi run --script`, `pixi init --script`, dependency editing, and shared `tool.pixi` support across script formats.

##### Documentation

- Document `conda-script` files by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in

### [`v0.79.0`](https://redirect.github.com/prefix-dev/pixi/releases/tag/v0.79.0)

[Compare Source](https://redirect.github.com/prefix-dev/pixi/compare/v0.78.0...v0.79.0)

##### \[0.79.0] - 2026-09-03

##### ✨ Highlights

Pixi now has better support for RISC-V by setting the default virtual packages.

And we've extended the CLI to include `pixi install --script` and `pixi workspace dependencies add`.

##### Added

- Add `pixi workspace dependencies` CLI by [@&#8203;ruben-arts](https://redirect.github.com/ruben-arts) in
- Install script environments without running by [@&#8203;manzt](https://redirect.github.com/manzt) in
- Set `__glibc` default to 2.39 on linux-riscv64 by [@&#8203;pavelzw](https://redirect.github.com/pavelzw) in

##### Changed

- Restore static MSVC CRT in Windows builds by [@&#8203;baszalmstra](https://redirect.github.com/baszalmstra) in

##### Documentation

- Mention named platforms in pixi-pack by [@&#8203;pavelzw](https://redirect.github.com/pavelzw) in

##### Fixed

- Bump astral_async_http_range_reader to 0.11.1 by [@&#8203;baszalmstra](https://redirect.github.com/baszalmstra) in

### [`v0.78.0`](https://redirect.github.com/prefix-dev/pixi/releases/tag/v0.78.0)

[Compare Source](https://redirect.github.com/prefix-dev/pixi/compare/v0.77.1...v0.78.0)

##### \[0.78.0] - 2026-08-28

##### ✨ Highlights

- You can now use the CLI to add path dependencies with the following command:
  ```shell
  pixi add --path ~/dev/pixi pixi
  pixi add --path ~/dev/vinca vinca --pypi
  pixi add --path ~/dev/ros_ws/src/common/package.xml ros-jazzy-common
  ```
- Pixi now can also share the configuration with rattler-based tools like `rattler-build` and `rattler-index`.
- The PEP 723 script support has been improved with better caching and resolution of scripts.

##### Added

- Write manifest platform names instead of pN aliases by [@&#8203;ruben-arts](https://redirect.github.com/ruben-arts) in
- Rework the terminal output by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Read the shared config layer of rattler-based tools by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Cache and update script resolutions by [@&#8203;manzt](https://redirect.github.com/manzt) in
- Honor the shared `index-config` when indexing a channel by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Add `--path` to the `pixi add` command by [@&#8203;ruben-arts](https://redirect.github.com/ruben-arts) in

##### Changed

- Solve scripts against the host's virtual packages by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Replace the distro index fetch with a static mapping by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Drop unused `InstallChange` helpers by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Pin the ROS fixture's macOS compiler to unbreak CI by [@&#8203;baszalmstra](https://redirect.github.com/baszalmstra) in

##### Fixed

- Group legacy purl fallback traces by [@&#8203;baszalmstra](https://redirect.github.com/baszalmstra) in
- Solve lockless scripts in `pixi workspace platform` by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Show progress for `pixi add` and other commands that solve by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in

### [`v0.77.1`](https://redirect.github.com/prefix-dev/pixi/releases/tag/v0.77.1)

[Compare Source](https://redirect.github.com/prefix-dev/pixi/compare/v0.77.0...v0.77.1)

##### \[0.77.1] - 2026-08-24

##### Added

- Add `pixi workspace preview` subcommand by [@&#8203;ruben-arts](https://redirect.github.com/ruben-arts) in
- Update rattler, rattler-build, minijinja, and display channel notices by [@&#8203;wolfv](https://redirect.github.com/wolfv) in
- Add one consistent API for detecting host machine by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in

##### Changed

- Use .mojoc with precompile by [@&#8203;sstadick](https://redirect.github.com/sstadick) in
- Render lists in the generated CLI docs by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in

##### Documentation

- Fix lists that render as plain text by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Add a social card for link previews by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in

##### Fixed

- Remove backends channel from pixi repo by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Avoid rebuilds for mtime-only source changes by [@&#8203;baszalmstra](https://redirect.github.com/baszalmstra) in

### [`v0.77.0`](https://redirect.github.com/prefix-dev/pixi/releases/tag/v0.77.0)

[Compare Source](https://redirect.github.com/prefix-dev/pixi/compare/v0.76.2...v0.77.0)

##### \[0.77.0] - 2026-08-18

##### ✨ Highlights

This release includes the API update for the backends such that we support `pin-compatible` and `pin-subpackage` specs in package manifests. For example:

```toml

# Pin subpackage `mypkg` to the same version as the package itself, but allow any compatible version of `mypkg` to be used.
[package]
name = "mypkg"

[package.run-exports.weak]
mypkg = { pin-subpackage = { upper-bound = "x.x" } }

# Pin `boltons` to a compatible version.
[package.host-dependencies]
boltons = ">=2,<3"

[package.run-dependencies]
boltons = { pin-compatible = true }
```

Note that this is a `pixi-build-api-version` update to `v7`.

Also, you can now modify the workspace activation with the new `pixi workspace activation` subcommand.

##### Added

- Pixi search with wildcard by default by [@&#8203;ruben-arts](https://redirect.github.com/ruben-arts) in
- Add `pixi workspace activation ...` cli subcommand. by [@&#8203;ruben-arts](https://redirect.github.com/ruben-arts) in
- Honor CONDA_OVERRIDE_ARCHSPEC by [@&#8203;hunger](https://redirect.github.com/hunger) in
- Support `pin-compatible` and `pin-subpackage` specs in package manifests by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in

##### Changed

- Saturate out-of-range cutoffs instead of panicking by [@&#8203;zach-marto](https://redirect.github.com/zach-marto) in

##### Documentation

- Suggest constraints not dependencies by [@&#8203;C-nit](https://redirect.github.com/C-nit) in
- Fix rich platform claim by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in

##### Fixed

- Dedup paths yielded by the glob walk by [@&#8203;ruben-arts](https://redirect.github.com/ruben-arts) in
- Treat corrupt task-cache JSON as a cache miss by [@&#8203;myukitty](https://redirect.github.com/myukitty) in
- Honor CONDA_OVERRIDE\_\* in the publish build and host solves by [@&#8203;bretttully](https://redirect.github.com/bretttully) in
- Help users with better help messages by [@&#8203;ruben-arts](https://redirect.github.com/ruben-arts) in
- Scope target activation to the platform a run targets by [@&#8203;ruben-arts](https://redirect.github.com/ruben-arts) in
- Enable `pixi_build_types` feature on `pixi_spec` in type conversions by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in

##### Refactor

- Use matchspecs for requirements in rich platforms by [@&#8203;hunger](https://redirect.github.com/hunger) in

##### New Contributors

- [@&#8203;Timple](https://redirect.github.com/Timple) made their first contribution in
- [@&#8203;randyzwitch](https://redirect.github.com/randyzwitch) made their first contribution in
- [@&#8203;bretttully](https://redirect.github.com/bretttully) made their first contribution in
- [@&#8203;myukitty](https://redirect.github.com/myukitty) made their first contribution in
- [@&#8203;zach-marto](https://redirect.github.com/zach-marto) made their first contribution in
- [@&#8203;C-nit](https://redirect.github.com/C-nit) made their first contribution in

### [`v0.76.2`](https://redirect.github.com/prefix-dev/pixi/releases/tag/v0.76.2)

[Compare Source](https://redirect.github.com/prefix-dev/pixi/compare/v0.76.1...v0.76.2)

##### \[0.76.2] - 2026-08-10

##### ✨ Highlights

This release features better logging and a few bug fixes.

##### Added

- Add logging about why pixi build has a cache miss by [@&#8203;baszalmstra](https://redirect.github.com/baszalmstra) in

##### Changed

- Log CEP-42 user-order conflicts at debug level by [@&#8203;hunger](https://redirect.github.com/hunger) in
- Small fixes to better handle paths containing spaces by [@&#8203;hunger](https://redirect.github.com/hunger) in

##### Documentation

- Split `prepare-docs` into tasks with dependencies by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Better organize the CLI nav sections by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Clarify pixi search channels by [@&#8203;ytausch](https://redirect.github.com/ytausch) in

##### Fixed

- Wasm example by [@&#8203;wolfv](https://redirect.github.com/wolfv) in
- Scope the build source check to the package that declares it by [@&#8203;baszalmstra](https://redirect.github.com/baszalmstra) in

##### Removed

- Remove stray `py-pixi-build-backend` `Cargo.lock` by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in

##### Style

- Use TOML 1.1 multi-line inline tables in `pixi.toml` by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in

### [`v0.76.1`](https://redirect.github.com/prefix-dev/pixi/releases/tag/v0.76.1)

[Compare Source](https://redirect.github.com/prefix-dev/pixi/compare/v0.76.0...v0.76.1)

##### \[0.76.1] - 2026-08-04

##### ✨ Highlights

This release features an improved `pixi task list` and a revamped docs page based on Zensical.

##### Changed

- Correct `task list` runnability and show every task by [@&#8203;hunger](https://redirect.github.com/hunger) in

##### Documentation

- Switch from mkdocs to zensical by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in

##### Fixed

- Honour `editable = true` in transitive `[tool.uv.sources]` at install time by [@&#8203;earlybard](https://redirect.github.com/earlybard) in

##### New Contributors

- [@&#8203;earlybard](https://redirect.github.com/earlybard) made their first contribution in

### [`v0.76.0`](https://redirect.github.com/prefix-dev/pixi/releases/tag/v0.76.0)

[Compare Source](https://redirect.github.com/prefix-dev/pixi/compare/v0.75.0...v0.76.0)

##### \[0.76.0] - 2026-08-03

##### ✨ Highlights

A single Python file can now carry its own environment. Pixi reads [PEP 723](https://packaging.python.org/en/latest/specifications/inline-script-metadata/) inline metadata, and `--script` works on `init`, `run`, `add`, `remove`, `lock`, `list`, `tree`, and `workspace export`.

```python title="hello.py"

# /// script
# requires-python = ">=3.11"

# dependencies = ["rich>=13.9.2"]
# ///
import rich

rich.print("Hi from [bold magenta]hello.py[/bold magenta]!")
```

`pixi run --script hello.py` is the whole setup: no `pixi.toml`, no `.pixi` folder, and no lock file unless you ask for one with `pixi lock --script`.

PEP 723 only covers Python and PyPI, so Pixi reads `tool.pixi` for the rest. This greatly extends the script's capabilities, for example to add conda dependencies:

```python title="earthquakes.py"

# /// script
# dependencies = ["httpx"]

# [tool.pixi.workspace]
# channels = ["conda-forge"]

# [tool.pixi.dependencies]
# gdal = "*"

# ///
```

`pixi init --script`, `pixi add --script <file> gdal`, and `pixi add --script <file> --pypi httpx` write that block for you. Scripts don't have to be local either: `pixi run --script` takes a URL, a [Gist page](https://gist.github.com/ruben-arts/596390c215bdd7d2578e593b5309d68e) (that one builds CPython 3.16 from git first), or `-` to read from stdin.

Thanks [@&#8203;manzt](https://redirect.github.com/manzt) for building this out across five PRs, see the [standalone scripts docs](https://pixi.prefix.dev/v0.76.0/python/scripts/) for the rest. This release also adds [`[package.run-exports]`](https://pixi.prefix.dev/v0.76.0/reference/pixi_manifest/#run-exports), an `lfs` field for git dependencies, and configurable channel priority.

##### Added

- Support PEP 723 scripts with `--script` by [@&#8203;manzt](https://redirect.github.com/manzt) in
- Support PEP 723 scripts in `pixi list` by [@&#8203;manzt](https://redirect.github.com/manzt) in
- Support `[package.run-exports]` in package manifests by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Run PEP 723 scripts from URLs by [@&#8203;manzt](https://redirect.github.com/manzt) in
- Resolve GitHub Gist scripts by [@&#8203;manzt](https://redirect.github.com/manzt) in
- Run PEP 723 scripts from standard input by [@&#8203;manzt](https://redirect.github.com/manzt) in
- Update rattler and support flexible channel priority by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Add `lfs` field to git dependencies by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in

##### Changed

- Point the uv cache at the pixi cache directory by [@&#8203;wolfv](https://redirect.github.com/wolfv) in
- Resolve the target subdir to the declared workspace platform by [@&#8203;wolfv](https://redirect.github.com/wolfv) in
- Strip ANSI escapes from rendered diagnostics in snapshots by [@&#8203;ruben-arts](https://redirect.github.com/ruben-arts) in

##### Documentation

- Make more use of the rich platform syntax by [@&#8203;ruben-arts](https://redirect.github.com/ruben-arts) in

##### Fixed

- Force reinstall with setuptools in ros backend by [@&#8203;ruben-arts](https://redirect.github.com/ruben-arts) in
- Fix pre-commit during backends release script by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in

##### New Contributors

- [@&#8203;manzt](https://redirect.github.com/manzt) made their first contribution in

### [`v0.75.0`](https://redirect.github.com/prefix-dev/pixi/releases/tag/v0.75.0)

[Compare Source](https://redirect.github.com/prefix-dev/pixi/compare/v0.74.0...v0.75.0)

##### \[0.75.0] - 2026-07-29

##### ✨ Highlights

[`pixi publish`](https://pixi.prefix.dev/v0.75.0/build/workspace/#publishing-the-workspace) now publishes every workspace package that opts in in dependency order. It also refuses to publish anything if a necessary source dependencies didn't opt into being published.

```toml title="packages/cpp_math/pixi.toml"
[package]
name = "cpp_math"
publish = true
```

[Offline solves](https://pixi.prefix.dev/v0.75.0/reference/pixi_configuration/#offline) now only consider conda packages you already have, either in the package cache or on a local `file://` channel.

That means that you can run `pixi update --offline` and it will resolve with the packages you have local on your machine.

Completion is now generated from the CLI itself, so environment, platform, feature, and task names complete on every option that takes one, in bash, zsh, fish, and nushell.

As usual we also fixed a couple of bugs.

##### Added

- Publish the workspace packages that opt in by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Rework completion by [@&#8203;hunger](https://redirect.github.com/hunger) in
- Restrict offline solves to locally available packages by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Validate \_\_archspec microarchitecture names by [@&#8203;hunger](https://redirect.github.com/hunger) in

##### Changed

- Report the assumed platform per environment by [@&#8203;hunger](https://redirect.github.com/hunger) in
- Reuse cached artifacts for immutable sources when the cache dir is gone by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in

##### Documentation

- Explain that `$PREFIX` is not expanded on Windows by [@&#8203;baszalmstra](https://redirect.github.com/baszalmstra) in

##### Fixed

- Clean stale files from incremental ROS Python builds by [@&#8203;baszalmstra](https://redirect.github.com/baszalmstra) in
- Fix CI on main and slim down extra slow tests by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in

##### Removed

- Remove stray insta snapshot artifact by [@&#8203;hunger](https://redirect.github.com/hunger) in

### [`v0.74.0`](https://redirect.github.com/prefix-dev/pixi/releases/tag/v0.74.0)

[Compare Source](https://redirect.github.com/prefix-dev/pixi/compare/v0.73.0...v0.74.0)

##### \[0.74.0] - 2026-07-27

##### ✨ Highlights

Environments can now define content inline, no feature needed:

```toml
[environments.test.dependencies]
pytest = "*"

[environments.test-lower-bounds]
solve-strategy = "lowest-direct"
```

And `pixi global` can build tools from sources without a package manifest, just name the backend:

```shell
pixi global install --git https://github.com/BurntSushi/xsv.git --build-backend pixi-build-rust
```

We also fixed a bunch of bugs.

##### Added

- Inline environments by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Add offline mode (--offline) by [@&#8203;baszalmstra](https://redirect.github.com/baszalmstra) in
- Inline package manifests for `pixi global` by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Fix build variant information for building by [@&#8203;ruben-arts](https://redirect.github.com/ruben-arts) in
- Add run_exports to Package by [@&#8203;samrosenf](https://redirect.github.com/samrosenf) in
- Add progress spinner to pixi global sync by [@&#8203;MannXo](https://redirect.github.com/MannXo) in
- Add --from-url option for self-update by [@&#8203;xia-tian-wu](https://redirect.github.com/xia-tian-wu) in

##### Changed

- Stop docs-dev racing docs-release on tag dispatch by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Keep PR docs builds out of the deploy concurrency group by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in

##### Documentation

- Typo in documentation by [@&#8203;anthonyylee](https://redirect.github.com/anthonyylee) in
- Fix examples to use `requires-pixi` instead of `pixi-minimum` by [@&#8203;li-em](https://redirect.github.com/li-em) in

##### Fixed

- Release workspace gateway before indexing on publish by [@&#8203;hunger](https://redirect.github.com/hunger) in
- Account for Cargo build target in install-as by [@&#8203;baszalmstra](https://redirect.github.com/baszalmstra) in
- Resolve changelog preview range end to a commit SHA by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Detect host/build dependency changes in `--check`/`--dry-run` by [@&#8203;baszalmstra](https://redirect.github.com/baszalmstra) in
- Prevent pytest-temp from accumulating stale run directories by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Better messaging when adding dependencies to unused features by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Name the failing environment in pixi global error messages by [@&#8203;tdejager](https://redirect.github.com/tdejager) in
- Rename `--subdir` to `--subdirectory` for git dependencies by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Only source declarations decide inline package suppression by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Name the package when solving build environments fails by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Give inline packages a name and a helpful error for Cargo workspaces by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Resolve rich platforms in `workspace export conda-explicit-spec` by [@&#8203;MridulS](https://redirect.github.com/MridulS) in

##### New Contributors

- [@&#8203;xia-tian-wu](https://redirect.github.com/xia-tian-wu) made their first contribution in
- [@&#8203;MannXo](https://redirect.github.com/MannXo) made their first contribution in
- [@&#8203;li-em](https://redirect.github.com/li-em) made their first contribution in
- [@&#8203;anthonyylee](https://redirect.github.com/anthonyylee) made their first contribution in

</details>

---

### Configuration

📅 **Schedule**: (in timezone America/Detroit)

- Branch creation
  - At any time (no schedule defined)
- Automerge
  - At any time (no schedule defined)

🚦 **Automerge**: Enabled.

♻ **Rebasing**: Whenever PR is behind base branch, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/glotzerlab/freud).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0NC4xNTUuMiIsInVwZGF0ZWRJblZlciI6IjQ0LjE1NS4yIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJkZXBlbmRlbmNpZXMiXX0=-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `730ce99571d0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
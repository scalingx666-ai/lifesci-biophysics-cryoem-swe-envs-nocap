# Issue

**Title:** Update pypa/cibuildwheel action to v4

## Description

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [pypa/cibuildwheel](https://redirect.github.com/pypa/cibuildwheel) | action | major | `v3.4.1` → `v4.0.0` |

---

### Release Notes

<details>
<summary>pypa/cibuildwheel (pypa/cibuildwheel)</summary>

### [`v4.0.0`](https://redirect.github.com/pypa/cibuildwheel/releases/tag/v4.0.0)

[Compare Source](https://redirect.github.com/pypa/cibuildwheel/compare/v4.0.0...v4.0.0)

See [@&#8203;henryiii](https://redirect.github.com/henryiii)'s [release post](https://iscinumpy.dev/post/cibuildwheel-4-0-0/) for more info on new features!

- 🌟 Adds wheel auditing with `abi3audit` as a default after the repair step, with new [`audit-requires`](https://cibuildwheel.pypa.io/en/stable/options/#audit-requires) and [`audit-command`](https://cibuildwheel.pypa.io/en/stable/options/#audit-command) options ()

- 🌟 Adds `pyemscripten` platform tag support (PEP 783), updates Pyodide to 314.0.0a2, and adds a `pyodide-eol` [`enable`](https://cibuildwheel.pypa.io/en/stable/options/#enable) flag for building end-of-life Pyodide versions (, )

- 🌟 Sets up `delvewheel` as the default [`repair-wheel-command`](https://cibuildwheel.pypa.io/en/stable/options/#repair-wheel-command) for Windows, so extension module DLLs are now bundled automatically. Skip by setting it to empty if not needed. ()

- ✨ Adds CPython 3.15 support, under the [`enable` option](https://cibuildwheel.pypa.io/en/stable/options/#enable) `cpython-prerelease`. This version of cibuildwheel uses 3.15.0b2. (, )

  *While CPython is in beta, the ABI can change, so your wheels might not be compatible with the final release. For this reason, we don't recommend distributing wheels until RC1, at which point 3.15 will be available in cibuildwheel without the flag.*

- ✨ Adds CPython 3.15 support for iOS and Android (, )

- ✨ Adds Android improvements for building NumPy and related packages, including auditwheel support, pkg-config and Fortran configuration, and the [`xbuild-files`](https://cibuildwheel.pypa.io/en/stable/options/#xbuild-files) option ()

- ✨ Adds `CIBUILDWHEEL_BUILD_IDENTIFIER` environment variable set to the current build identifier (e.g. `cp311-manylinux_x86_64`) during per-build steps ()

- ✨ Adds `{project}` and `{package}` placeholders to [`config-settings`](https://cibuildwheel.pypa.io/en/stable/options/#config-settings) ()

- ⚠️ Drops support for Python 3.8 ()

- ⚠️ Removes the experimental CPython 3.13 free-threading builds and the `cpython-freethreading` [`enable`](https://cibuildwheel.pypa.io/en/stable/options/#enable) option. CPython 3.14+ free-threading support remains available without the enable flag. ()

- ⚠️ Drops support for Cirrus CI, which is shutting down June 1, 2026 ()

- ⚠️ Drops GraalPy 3.11 (gp311) support, as agreed in , and removes GraalPy 24-only workarounds ()

- 🔐 Adds SHA256 verification for direct downloads of Python interpreters, virtualenv, and python-build-standalone assets ()

- 🔐 Adds tarfile extraction filter for safe archive extraction ()

- 🐛 Fixes `UV_PYTHON` not being set for [`before-build`](https://cibuildwheel.pypa.io/en/stable/options/#before-build) on Linux when using `uv` as the [`build-frontend`](https://cibuildwheel.pypa.io/en/stable/options/#build-frontend) ()

- 🐛 Fixes detection of musl libc when downloading python-build-standalone, which previously always selected the gnu asset on musl hosts like Alpine ()

- 🐛 Fixes [`config-settings`](https://cibuildwheel.pypa.io/en/stable/options/#config-settings) expansion when `{project}` or `{package}` contains spaces or backslashes ()

- 🐛 Prevents deadlock when `linux32` fails and forwards platform args to the sanity check (, )

- 🐛 Fixes container resource leaks on start failure and during teardown (, )

- 🐛 Removes potential partial cache-population in case of error ()

- 🐛 Raises a clear error when `ANDROID_API_LEVEL` is not an integer ()

- 🐛 Replaces assert with proper exception in python-build-standalone ()

- 🐛 Uses ConfigurationError when `package_dir` is outside cwd instead of a generic Exception ()

- 🛠 Updates dependencies and container pins (, , , , , , , , , , , )

- 🛠 Updates Android to Python 3.13.13 and 3.14.4 ()

- 🛠 Applies Pyodide-specific patches to the Emscripten toolchain installation ()

- 🛠 Uses `python -V -V` for Windows build diagnostics ()

- 🛠 Simplifies pinned container image lookup ()

- 🛠 Minor fixups across error messages, OCI container, and options ()

- 💼 Adds PEP 723 metadata for `bin/` scripts and drops the `bin` dependency group ()

- 💼 Improves Azure test reliability with retries and caching ()

- 💼 Fixes Windows GitLab CI test running ()

- 💼 Updates CI action pins and dev dependencies (, , , , , , , )

- 💼 Adds agent and copilot setup files ()

- 💼 Uses `if TYPE_CHECKING:` blocks (, )

- 🧪 Fixes Android tests using the `uv` frontend ()

- 🧪 Fixes the update-dependencies workflow to use `uv` to run `nox` ()

- 🧪 Adds unit tests for `OCIContainer._get_platform_args` ()

- 📚 Updates documentation for delvewheel as the default Windows [`repair-wheel-command`](https://cibuildwheel.pypa.io/en/stable/options/#repair-wheel-command), including the build diagram, schema defaults, and legal note (, , )

- 📚 Documents platform-specific [`before-build`](https://cibuildwheel.pypa.io/en/stable/options/#before-build) configuration ()

- 📚 Updates the "How it works" diagram with details of Android, iOS, and Pyodide builds ()

- 📚 Adds Pyodide icon and regenerates working examples data for Android, iOS, and Pyodide (, )

- 📚 Adds intersphinx support for external documentation linking ()

- 📚 Adds instructions for building CUDA wheels and fixes manylinux container references in FAQ (, )

- 📚 Links back to source in docs ()

- 📚 Removes outdated numpy info ()

### [`v4.0`](https://redirect.github.com/pypa/cibuildwheel/compare/v3.4.1...v4.0.0)

[Compare Source](https://redirect.github.com/pypa/cibuildwheel/compare/v3.4.1...v4.0.0)

</details>

---

### Configuration

📅 **Schedule**: (in timezone America/Detroit)

- Branch creation
  - Only on Monday (`* * * * 1`)
- Automerge
  - At any time (no schedule defined)

🚦 **Automerge**: Enabled.

♻ **Rebasing**: Whenever PR is behind base branch, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/glotzerlab/freud).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0My4yMDkuNCIsInVwZGF0ZWRJblZlciI6IjQzLjIwOS40IiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJkZXBlbmRlbmNpZXMiXX0=-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `60675011611c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Update dependency prek to >=0.5.3,<0.6

## Description

This PR contains the following updates:

| Package | Update | Change |
|---|---|---|
| [prek](http://anaconda.org/conda-forge/prek) ([source](https://redirect.github.com/j178/prek)) | minor | `>=0.4.6,<0.5` → `>=0.5.3,<0.6` |

---

### Release Notes

<details>
<summary>j178/prek (prek)</summary>

### [`v0.5.3`](https://redirect.github.com/j178/prek/blob/HEAD/CHANGELOG.md#053)

[Compare Source](https://redirect.github.com/j178/prek/compare/v0.5.2...v0.5.3)

Released on 2026-09-13.

##### Enhancements

- Add PEP 740 attestations for PyPI releases ()
- Add a `check-jsonc` builtin hook ()
- Allow disabling automatic uv installation ()

##### Bug fixes

- Fix Julia additional dependency specifiers ()
- Update `granit-parser` to fix YAML flow indentation ()

##### Contributors

- [@&#8203;clbarnes](https://redirect.github.com/clbarnes)
- [@&#8203;j178](https://redirect.github.com/j178)
- [@&#8203;tisonkun](https://redirect.github.com/tisonkun)

### [`v0.5.2`](https://redirect.github.com/j178/prek/blob/HEAD/CHANGELOG.md#052)

[Compare Source](https://redirect.github.com/j178/prek/compare/v0.5.1...v0.5.2)

Released on 2026-09-03.

##### Enhancements

- Allow unknown tags by default in `check-yaml` ()

##### Contributors

- [@&#8203;j178](https://redirect.github.com/j178)

### [`v0.5.1`](https://redirect.github.com/j178/prek/blob/HEAD/CHANGELOG.md#051)

[Compare Source](https://redirect.github.com/j178/prek/compare/v0.5.0...v0.5.1)

Released on 2026-09-01.

##### Enhancements

- Add `--hide-status <passed|failed|skipped>` for hook reports ()
- Add `prek init` for repository setup ()
- Apply hook `env` during environment creation ()
- Disable error snippets in `check-yaml` diagnostics ()
- Show hooks excluded by skip selectors ()
- Support Pixi for Conda environments ()
- Support `cargo-binstall` for Rust CLI dependencies ()
- Warn about unused keys in user settings ()

##### Bug fixes

- Reject unsupported YAML tags in check-yaml ()

##### Documentation

- Clarify local hook documentation ()
- Clarify pre-commit command compatibility ()
- Document automatic PR fixes with autofix.ci ()
- Document check-yaml unsafe support ()
- Improve setup and workflow documentation ()

##### Other changes

- Drop low-usage release targets ()
- Generate a prek manifest JSON schema ()

##### Contributors

- [@&#8203;j178](https://redirect.github.com/j178)

### [`v0.5.0`](https://redirect.github.com/j178/prek/blob/HEAD/CHANGELOG.md#050)

[Compare Source](https://redirect.github.com/j178/prek/compare/v0.4.14...v0.5.0)

Released on 2026-08-27.

##### Highlights

##### Choose where hook toolchains come from

`language_version` now accepts a source `preference` alongside the version
`request`, letting you control where prek looks for a compatible toolchain when
it creates a hook environment. Use `managed` (the default) or `system` to choose
which source prek tries first while still allowing fallback and downloads. Use
`only-managed` or `only-system` to require one source.

For example, this local Ruff hook requires a Python 3.12 toolchain managed by
prek:

```yaml
repos:
  - repo: local
    hooks:
      - id: ruff
        name: ruff
        language: python
        entry: ruff check
        additional_dependencies: [ruff]
        language_version:
          request: "3.12"
          preference: only-managed
```

With `only-managed`, prek reuses a compatible toolchain from its managed store
or downloads one when needed. It never falls back to Python from `PATH`, an OS
package manager, or a version manager, so toolchain selection does not depend on
the developer or CI machine's external environment.

Existing scalar values such as `language_version: "3.12"` continue to work. See
[toolchain management and `language_version`](https://prek.j178.dev/0.5.0/languages/#toolchain-management-and-language_version)
for the full source-selection behavior. ()

##### Breaking changes

The breaking changes in this release are mostly small cleanups, and most users should not be affected.

- Group names can no longer start with `@`. This prefix is now reserved for special group selectors such as the new `@ungrouped` selector. ()
- `PREK_MAX_CONCURRENCY` has been removed. Use `PREK_CONCURRENT_HOOKS` and `PREK_CONCURRENT_BATCHES` to control hook and per-hook batch concurrency separately. ()
- The top-level `prek init-template-dir` command has been removed. Use `prek util init-template-dir`, or `prek init-templatedir` for drop-in compatibility with `pre-commit`. ()
- `prek auto-update` has been removed. Use `prek update`, or `prek autoupdate` for drop-in compatibility with `pre-commit`. ()

##### Enhancements

- Add configurable toolchain source preferences ()
- Allow explicit `language: python` to opt out of fast-path hooks ()
- Group `run` and `update` options in CLI help ()
- Preserve terminal theme contrast for status labels ()
- Add `@ungrouped` group selector ()
- Retry uv installation with static musl binary ()
- Support `{hook_repo}` placeholder in hook `entry` ()

##### Bug fixes

- Expose the git work tree to hooks running outside the git root ()
- Isolate local hook environment installation ()
- Preserve hook config deserialization error locations ()
- Resolve hook log files from their config directory ()
- Track explicit config paths as absolute paths ()

##### Documentation

- Document prek run architecture ()
- Publish versioned documentation ()

##### Contributors

- [@&#8203;jeromeheissler](https://redirect.github.com/jeromeheissler)
- [@&#8203;gurgeous](https://redirect.github.com/gurgeous)
- [@&#8203;j178](https://redirect.github.com/j178)

</details>

---

### Configuration

📅 **Schedule**: (in timezone America/Detroit)

- Branch creation
  - At any time (no schedule defined)
- Automerge
  - At any time (no schedule defined)

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/glotzerlab/freud).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0NC45NC4wIiwidXBkYXRlZEluVmVyIjoiNDQuOTQuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiZGVwZW5kZW5jaWVzIl19-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e4c55bc85c41`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
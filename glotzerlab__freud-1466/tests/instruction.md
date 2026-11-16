# Issue

**Title:** Update dependency prek to v0.5.3

## Description

> ℹ️ **Note**
>
> This PR body was truncated due to platform limits.

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [prek](https://redirect.github.com/j178/prek) | uses-with | minor | `0.3.8` → `v0.5.3` |

---

### Release Notes

<details>
<summary>j178/prek (prek)</summary>

### [`v0.5.3`](https://redirect.github.com/j178/prek/releases/tag/v0.5.3): 0.5.3

[Compare Source](https://redirect.github.com/j178/prek/compare/v0.5.2...v0.5.3)

#### Release Notes

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

#### Install prek 0.5.3

##### Install prebuilt binaries via shell script

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/j178/prek/releases/download/v0.5.3/prek-installer.sh | sh
```

##### Install prebuilt binaries via powershell script

```sh
powershell -ExecutionPolicy Bypass -c "irm https://github.com/j178/prek/releases/download/v0.5.3/prek-installer.ps1 | iex"
```

##### Install prebuilt binaries via Homebrew

```sh
brew install prek
```

#### Download prek 0.5.3

| File                                                                                                                                   | Platform            | Checksum                                                                                                        |
| -------------------------------------------------------------------------------------------------------------------------------------- | ------------------- | --------------------------------------------------------------------------------------------------------------- |
| [prek-aarch64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.3/prek-aarch64-apple-darwin.tar.gz)             | Apple Silicon macOS | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.3/prek-aarch64-apple-darwin.tar.gz.sha256)       |
| [prek-x86\_64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.3/prek-x86_64-apple-darwin.tar.gz)              | Intel macOS         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.3/prek-x86_64-apple-darwin.tar.gz.sha256)        |
| [prek-aarch64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.5.3/prek-aarch64-pc-windows-msvc.zip)             | ARM64 Windows       | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.3/prek-aarch64-pc-windows-msvc.zip.sha256)       |
| [prek-x86\_64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.5.3/prek-x86_64-pc-windows-msvc.zip)              | x64 Windows         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.3/prek-x86_64-pc-windows-msvc.zip.sha256)        |
| [prek-aarch64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.3/prek-aarch64-unknown-linux-gnu.tar.gz)   | ARM64 Linux         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.3/prek-aarch64-unknown-linux-gnu.tar.gz.sha256)  |
| [prek-x86\_64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.3/prek-x86_64-unknown-linux-gnu.tar.gz)    | x64 Linux           | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.3/prek-x86_64-unknown-linux-gnu.tar.gz.sha256)   |
| [prek-aarch64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.3/prek-aarch64-unknown-linux-musl.tar.gz) | ARM64 MUSL Linux    | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.3/prek-aarch64-unknown-linux-musl.tar.gz.sha256) |
| [prek-x86\_64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.3/prek-x86_64-unknown-linux-musl.tar.gz)  | x64 MUSL Linux      | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.3/prek-x86_64-unknown-linux-musl.tar.gz.sha256)  |

#### Verifying GitHub Artifact Attestations

The artifacts in this release have attestations generated with GitHub Artifact Attestations. These can be verified by using the [GitHub CLI](https://cli.github.com/manual/gh_attestation_verify):

```sh
gh attestation verify <file-path of downloaded artifact> --repo j178/prek
```

You can also download the attestation from [GitHub](https://redirect.github.com/j178/prek/attestations) and verify against that directly:

```sh
gh attestation verify <file-path of downloaded artifact> --bundle <file-path of downloaded attestation>
```

### [`v0.5.2`](https://redirect.github.com/j178/prek/releases/tag/v0.5.2): 0.5.2

[Compare Source](https://redirect.github.com/j178/prek/compare/v0.5.1...v0.5.2)

#### Release Notes

Released on 2026-09-03.

##### Enhancements

- Allow unknown tags by default in `check-yaml` ()

##### Contributors

- [@&#8203;j178](https://redirect.github.com/j178)

#### Install prek 0.5.2

##### Install prebuilt binaries via shell script

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/j178/prek/releases/download/v0.5.2/prek-installer.sh | sh
```

##### Install prebuilt binaries via powershell script

```sh
powershell -ExecutionPolicy Bypass -c "irm https://github.com/j178/prek/releases/download/v0.5.2/prek-installer.ps1 | iex"
```

##### Install prebuilt binaries via Homebrew

```sh
brew install prek
```

#### Download prek 0.5.2

| File                                                                                                                                   | Platform            | Checksum                                                                                                        |
| -------------------------------------------------------------------------------------------------------------------------------------- | ------------------- | --------------------------------------------------------------------------------------------------------------- |
| [prek-aarch64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.2/prek-aarch64-apple-darwin.tar.gz)             | Apple Silicon macOS | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.2/prek-aarch64-apple-darwin.tar.gz.sha256)       |
| [prek-x86\_64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.2/prek-x86_64-apple-darwin.tar.gz)              | Intel macOS         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.2/prek-x86_64-apple-darwin.tar.gz.sha256)        |
| [prek-aarch64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.5.2/prek-aarch64-pc-windows-msvc.zip)             | ARM64 Windows       | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.2/prek-aarch64-pc-windows-msvc.zip.sha256)       |
| [prek-x86\_64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.5.2/prek-x86_64-pc-windows-msvc.zip)              | x64 Windows         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.2/prek-x86_64-pc-windows-msvc.zip.sha256)        |
| [prek-aarch64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.2/prek-aarch64-unknown-linux-gnu.tar.gz)   | ARM64 Linux         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.2/prek-aarch64-unknown-linux-gnu.tar.gz.sha256)  |
| [prek-x86\_64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.2/prek-x86_64-unknown-linux-gnu.tar.gz)    | x64 Linux           | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.2/prek-x86_64-unknown-linux-gnu.tar.gz.sha256)   |
| [prek-aarch64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.2/prek-aarch64-unknown-linux-musl.tar.gz) | ARM64 MUSL Linux    | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.2/prek-aarch64-unknown-linux-musl.tar.gz.sha256) |
| [prek-x86\_64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.2/prek-x86_64-unknown-linux-musl.tar.gz)  | x64 MUSL Linux      | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.2/prek-x86_64-unknown-linux-musl.tar.gz.sha256)  |

#### Verifying GitHub Artifact Attestations

The artifacts in this release have attestations generated with GitHub Artifact Attestations. These can be verified by using the [GitHub CLI](https://cli.github.com/manual/gh_attestation_verify):

```sh
gh attestation verify <file-path of downloaded artifact> --repo j178/prek
```

You can also download the attestation from [GitHub](https://redirect.github.com/j178/prek/attestations) and verify against that directly:

```sh
gh attestation verify <file-path of downloaded artifact> --bundle <file-path of downloaded attestation>
```

### [`v0.5.1`](https://redirect.github.com/j178/prek/releases/tag/v0.5.1): 0.5.1

[Compare Source](https://redirect.github.com/j178/prek/compare/v0.5.0...v0.5.1)

#### Release Notes

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

#### Install prek 0.5.1

##### Install prebuilt binaries via shell script

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/j178/prek/releases/download/v0.5.1/prek-installer.sh | sh
```

##### Install prebuilt binaries via powershell script

```sh
powershell -ExecutionPolicy Bypass -c "irm https://github.com/j178/prek/releases/download/v0.5.1/prek-installer.ps1 | iex"
```

##### Install prebuilt binaries via Homebrew

```sh
brew install prek
```

#### Download prek 0.5.1

| File                                                                                                                                   | Platform            | Checksum                                                                                                        |
| -------------------------------------------------------------------------------------------------------------------------------------- | ------------------- | --------------------------------------------------------------------------------------------------------------- |
| [prek-aarch64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.1/prek-aarch64-apple-darwin.tar.gz)             | Apple Silicon macOS | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.1/prek-aarch64-apple-darwin.tar.gz.sha256)       |
| [prek-x86\_64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.1/prek-x86_64-apple-darwin.tar.gz)              | Intel macOS         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.1/prek-x86_64-apple-darwin.tar.gz.sha256)        |
| [prek-aarch64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.5.1/prek-aarch64-pc-windows-msvc.zip)             | ARM64 Windows       | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.1/prek-aarch64-pc-windows-msvc.zip.sha256)       |
| [prek-x86\_64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.5.1/prek-x86_64-pc-windows-msvc.zip)              | x64 Windows         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.1/prek-x86_64-pc-windows-msvc.zip.sha256)        |
| [prek-aarch64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.1/prek-aarch64-unknown-linux-gnu.tar.gz)   | ARM64 Linux         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.1/prek-aarch64-unknown-linux-gnu.tar.gz.sha256)  |
| [prek-x86\_64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.1/prek-x86_64-unknown-linux-gnu.tar.gz)    | x64 Linux           | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.1/prek-x86_64-unknown-linux-gnu.tar.gz.sha256)   |
| [prek-aarch64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.1/prek-aarch64-unknown-linux-musl.tar.gz) | ARM64 MUSL Linux    | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.1/prek-aarch64-unknown-linux-musl.tar.gz.sha256) |
| [prek-x86\_64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.1/prek-x86_64-unknown-linux-musl.tar.gz)  | x64 MUSL Linux      | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.1/prek-x86_64-unknown-linux-musl.tar.gz.sha256)  |

#### Verifying GitHub Artifact Attestations

The artifacts in this release have attestations generated with GitHub Artifact Attestations. These can be verified by using the [GitHub CLI](https://cli.github.com/manual/gh_attestation_verify):

```sh
gh attestation verify <file-path of downloaded artifact> --repo j178/prek
```

You can also download the attestation from [GitHub](https://redirect.github.com/j178/prek/attestations) and verify against that directly:

```sh
gh attestation verify <file-path of downloaded artifact> --bundle <file-path of downloaded attestation>
```

### [`v0.5.0`](https://redirect.github.com/j178/prek/releases/tag/v0.5.0): 0.5.0

[Compare Source](https://redirect.github.com/j178/prek/compare/v0.4.14...v0.5.0)

#### Release Notes

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

#### Install prek 0.5.0

##### Install prebuilt binaries via shell script

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/j178/prek/releases/download/v0.5.0/prek-installer.sh | sh
```

##### Install prebuilt binaries via powershell script

```sh
powershell -ExecutionPolicy Bypass -c "irm https://github.com/j178/prek/releases/download/v0.5.0/prek-installer.ps1 | iex"
```

##### Install prebuilt binaries via Homebrew

```sh
brew install prek
```

#### Download prek 0.5.0

| File                                                                                                                                           | Platform                     | Checksum                                                                                                            |
| ---------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| [prek-aarch64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-aarch64-apple-darwin.tar.gz)                     | Apple Silicon macOS          | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-aarch64-apple-darwin.tar.gz.sha256)           |
| [prek-x86\_64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-x86_64-apple-darwin.tar.gz)                      | Intel macOS                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-x86_64-apple-darwin.tar.gz.sha256)            |
| [prek-aarch64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-aarch64-pc-windows-msvc.zip)                     | ARM64 Windows                | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-aarch64-pc-windows-msvc.zip.sha256)           |
| [prek-i686-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-i686-pc-windows-msvc.zip)                           | x86 Windows                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-i686-pc-windows-msvc.zip.sha256)              |
| [prek-x86\_64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-x86_64-pc-windows-msvc.zip)                      | x64 Windows                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-x86_64-pc-windows-msvc.zip.sha256)            |
| [prek-aarch64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-aarch64-unknown-linux-gnu.tar.gz)           | ARM64 Linux                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-aarch64-unknown-linux-gnu.tar.gz.sha256)      |
| [prek-i686-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-i686-unknown-linux-gnu.tar.gz)                 | x86 Linux                    | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-i686-unknown-linux-gnu.tar.gz.sha256)         |
| [prek-riscv64gc-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-riscv64gc-unknown-linux-gnu.tar.gz)       | RISCV Linux                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-riscv64gc-unknown-linux-gnu.tar.gz.sha256)    |
| [prek-s390x-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-s390x-unknown-linux-gnu.tar.gz)               | S390x Linux                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-s390x-unknown-linux-gnu.tar.gz.sha256)        |
| [prek-x86\_64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-x86_64-unknown-linux-gnu.tar.gz)            | x64 Linux                    | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-x86_64-unknown-linux-gnu.tar.gz.sha256)       |
| [prek-armv7-unknown-linux-gnueabihf.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-armv7-unknown-linux-gnueabihf.tar.gz)   | ARMv7 Linux                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-armv7-unknown-linux-gnueabihf.tar.gz.sha256)  |
| [prek-aarch64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-aarch64-unknown-linux-musl.tar.gz)         | ARM64 MUSL Linux             | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-aarch64-unknown-linux-musl.tar.gz.sha256)     |
| [prek-i686-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-i686-unknown-linux-musl.tar.gz)               | x86 MUSL Linux               | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-i686-unknown-linux-musl.tar.gz.sha256)        |
| [prek-x86\_64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-x86_64-unknown-linux-musl.tar.gz)          | x64 MUSL Linux               | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-x86_64-unknown-linux-musl.tar.gz.sha256)      |
| [prek-arm-unknown-linux-musleabihf.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-arm-unknown-linux-musleabihf.tar.gz)     | ARMv6 MUSL Linux (Hardfloat) | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-arm-unknown-linux-musleabihf.tar.gz.sha256)   |
| [prek-armv7-unknown-linux-musleabihf.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-armv7-unknown-linux-musleabihf.tar.gz) | ARMv7 MUSL Linux             | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.0/prek-armv7-unknown-linux-musleabihf.tar.gz.sha256) |

#### Verifying GitHub Artifact Attestations

The artifacts in this release have attestations generated with GitHub Artifact Attestations. These can be verified by using the [GitHub CLI](https://cli.github.com/manual/gh_attestation_verify):

```sh
gh attestation verify <file-path of downloaded artifact> --repo j178/prek
```

You can also download the attestation from [GitHub](https://redirect.github.com/j178/prek/attestations) and verify against that directly:

```sh
gh attestation verify <file-path of downloaded artifact> --bundle <file-path of downloaded attestation>
```

### [`v0.4.14`](https://redirect.github.com/j178/prek/releases/tag/v0.4.14): 0.4.14

[Compare Source](https://redirect.github.com/j178/prek/compare/v0.4.13...v0.4.14)

#### Release Notes

Released on 2026-08-17.

##### Enhancements

- Support check-yaml unsafe mode ()

##### Performance

- Reuse allocations in common filters ()

##### Bug fixes

- Accept non-finite floats in check-yaml ()
- Allow comment-heavy YAML in check-yaml ()
- Sanitize captured terminal output before replay ()

##### Contributors

- [@&#8203;j178](https://redirect.github.com/j178)

#### Install prek 0.4.14

##### Install prebuilt binaries via shell script

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/j178/prek/releases/download/v0.4.14/prek-installer.sh | sh
```

##### Install prebuilt binaries via powershell script

```sh
powershell -ExecutionPolicy Bypass -c "irm https://github.com/j178/prek/releases/download/v0.4.14/prek-installer.ps1 | iex"
```

##### Install prebuilt binaries via Homebrew

```sh
brew install prek
```

#### Download prek 0.4.14

| File                                                                                                                                            | Platform                     | Checksum                                                                                                             |
| ----------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| [prek-aarch64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-aarch64-apple-darwin.tar.gz)                     | Apple Silicon macOS          | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-aarch64-apple-darwin.tar.gz.sha256)           |
| [prek-x86\_64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-x86_64-apple-darwin.tar.gz)                      | Intel macOS                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-x86_64-apple-darwin.tar.gz.sha256)            |
| [prek-aarch64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-aarch64-pc-windows-msvc.zip)                     | ARM64 Windows                | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-aarch64-pc-windows-msvc.zip.sha256)           |
| [prek-i686-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-i686-pc-windows-msvc.zip)                           | x86 Windows                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-i686-pc-windows-msvc.zip.sha256)              |
| [prek-x86\_64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-x86_64-pc-windows-msvc.zip)                      | x64 Windows                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-x86_64-pc-windows-msvc.zip.sha256)            |
| [prek-aarch64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-aarch64-unknown-linux-gnu.tar.gz)           | ARM64 Linux                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-aarch64-unknown-linux-gnu.tar.gz.sha256)      |
| [prek-i686-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-i686-unknown-linux-gnu.tar.gz)                 | x86 Linux                    | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-i686-unknown-linux-gnu.tar.gz.sha256)         |
| [prek-riscv64gc-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-riscv64gc-unknown-linux-gnu.tar.gz)       | RISCV Linux                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-riscv64gc-unknown-linux-gnu.tar.gz.sha256)    |
| [prek-s390x-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-s390x-unknown-linux-gnu.tar.gz)               | S390x Linux                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-s390x-unknown-linux-gnu.tar.gz.sha256)        |
| [prek-x86\_64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-x86_64-unknown-linux-gnu.tar.gz)            | x64 Linux                    | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-x86_64-unknown-linux-gnu.tar.gz.sha256)       |
| [prek-armv7-unknown-linux-gnueabihf.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-armv7-unknown-linux-gnueabihf.tar.gz)   | ARMv7 Linux                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-armv7-unknown-linux-gnueabihf.tar.gz.sha256)  |
| [prek-aarch64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-aarch64-unknown-linux-musl.tar.gz)         | ARM64 MUSL Linux             | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-aarch64-unknown-linux-musl.tar.gz.sha256)     |
| [prek-i686-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-i686-unknown-linux-musl.tar.gz)               | x86 MUSL Linux               | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-i686-unknown-linux-musl.tar.gz.sha256)        |
| [prek-x86\_64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-x86_64-unknown-linux-musl.tar.gz)          | x64 MUSL Linux               | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-x86_64-unknown-linux-musl.tar.gz.sha256)      |
| [prek-arm-unknown-linux-musleabihf.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-arm-unknown-linux-musleabihf.tar.gz)     | ARMv6 MUSL Linux (Hardfloat) | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-arm-unknown-linux-musleabihf.tar.gz.sha256)   |
| [prek-armv7-unknown-linux-musleabihf.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-armv7-unknown-linux-musleabihf.tar.gz) | ARMv7 MUSL Linux             | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.14/prek-armv7-unknown-linux-musleabihf.tar.gz.sha256) |

#### Verifying GitHub Artifact Attestations

The artifacts in this release have attestations generated with GitHub Artifact Attestations. These can be verified by using the [GitHub CLI](https://cli.github.com/manual/gh_attestation_verify):

```sh
gh attestation verify <file-path of downloaded artifact> --repo j178/prek
```

You can also download the attestation from [GitHub](https://redirect.github.com/j178/prek/attestations) and verify against that directly:

```sh
gh attestation verify <file-path of downloaded artifact> --bundle <file-path of downloaded attestation>
```

### [`v0.4.13`](https://redirect.github.com/j178/prek/releases/tag/v0.4.13): 0.4.13

[Compare Source](https://redirect.github.com/j178/prek/compare/v0.4.12...v0.4.13)

#### Release Notes

Released on 2026-08-10.

##### Highlights

##### Manage hook tools with mise

The new `language: mise` support lets hooks install tools using
[`mise`](https://mise.jdx.dev/) in an isolated environment:

```yaml
repos:
  - repo: local
    hooks:
      - id: golangci-lint
        name: golangci-lint
        language: mise
        additional_dependencies: ["aqua:golangci/golangci-lint@2"]
        entry: golangci-lint run --fast-only ./...
        pass_filenames: false
```

##### Run commands in hook environments

The new `prek exec` subcommand can run an explicit command in a configured
hook's prepared environment. For example, the hook above makes its managed
binary available to this command:

```console
$ prek exec golangci-lint -- golangci-lint --version
```

##### Enhancements

- Add `mise` language support ()
- Add `deny-filename-pattern` and `require-filename-pattern` hooks ()
- Add `prek exec` for running commands in a hook environment ()
- Add `yaml-language-server:` comment to YAML sample config ()
- Make `prek cache size` output terminal-aware ()
- Match file regexes against path bytes ()
- Show hook aliases in run output ()
- Show hook descriptions in run output ()

##### Performance

- Avoid env cache scans for skipped hooks ()
- Cache Node version queries ()

##### Bug fixes

- Fix Node hook installation with npm 11 ()
- Isolate dependency installs from inherited Git repository state ()

##### Documentation

- Document combined group filtering ()
- Explain what prek does in README ()
- Refresh "Why prek" highlights ()

##### Contributors

- [@&#8203;will-wright-eng](https://redirect.github.com/will-wright-eng)
- [@&#8203;Repiteo](https://redirect.github.com/Repiteo)
- [@&#8203;j178](https://redirect.github.com/j178)

#### Install prek 0.4.13

##### Install prebuilt binaries via shell script

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/j178/prek/releases/download/v0.4.13/prek-installer.sh | sh
```

##### Install prebuilt binaries via powershell script

```sh
powershell -ExecutionPolicy Bypass -c "irm https://github.com/j178/prek/releases/download/v0.4.13/prek-installer.ps1 | iex"
```

##### Install prebuilt binaries via Homebrew

```sh
brew install prek
```

#### Download prek 0.4.13

| File                                                                                                                                            | Platform                     | Checksum                                                                                                             |
| ----------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| [prek-aarch64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-aarch64-apple-darwin.tar.gz)                     | Apple Silicon macOS          | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-aarch64-apple-darwin.tar.gz.sha256)           |
| [prek-x86\_64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-x86_64-apple-darwin.tar.gz)                      | Intel macOS                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-x86_64-apple-darwin.tar.gz.sha256)            |
| [prek-aarch64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-aarch64-pc-windows-msvc.zip)                     | ARM64 Windows                | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-aarch64-pc-windows-msvc.zip.sha256)           |
| [prek-i686-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-i686-pc-windows-msvc.zip)                           | x86 Windows                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-i686-pc-windows-msvc.zip.sha256)              |
| [prek-x86\_64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-x86_64-pc-windows-msvc.zip)                      | x64 Windows                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-x86_64-pc-windows-msvc.zip.sha256)            |
| [prek-aarch64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-aarch64-unknown-linux-gnu.tar.gz)           | ARM64 Linux                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-aarch64-unknown-linux-gnu.tar.gz.sha256)      |
| [prek-i686-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-i686-unknown-linux-gnu.tar.gz)                 | x86 Linux                    | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-i686-unknown-linux-gnu.tar.gz.sha256)         |
| [prek-riscv64gc-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-riscv64gc-unknown-linux-gnu.tar.gz)       | RISCV Linux                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-riscv64gc-unknown-linux-gnu.tar.gz.sha256)    |
| [prek-s390x-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-s390x-unknown-linux-gnu.tar.gz)               | S390x Linux                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-s390x-unknown-linux-gnu.tar.gz.sha256)        |
| [prek-x86\_64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-x86_64-unknown-linux-gnu.tar.gz)            | x64 Linux                    | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-x86_64-unknown-linux-gnu.tar.gz.sha256)       |
| [prek-armv7-unknown-linux-gnueabihf.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-armv7-unknown-linux-gnueabihf.tar.gz)   | ARMv7 Linux                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-armv7-unknown-linux-gnueabihf.tar.gz.sha256)  |
| [prek-aarch64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-aarch64-unknown-linux-musl.tar.gz)         | ARM64 MUSL Linux             | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-aarch64-unknown-linux-musl.tar.gz.sha256)     |
| [prek-i686-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-i686-unknown-linux-musl.tar.gz)               | x86 MUSL Linux               | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-i686-unknown-linux-musl.tar.gz.sha256)        |
| [prek-x86\_64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-x86_64-unknown-linux-musl.tar.gz)          | x64 MUSL Linux               | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-x86_64-unknown-linux-musl.tar.gz.sha256)      |
| [prek-arm-unknown-linux-musleabihf.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-arm-unknown-linux-musleabihf.tar.gz)     | ARMv6 MUSL Linux (Hardfloat) | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-arm-unknown-linux-musleabihf.tar.gz.sha256)   |
| [prek-armv7-unknown-linux-musleabihf.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-armv7-unknown-linux-musleabihf.tar.gz) | ARMv7 MUSL Linux             | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.13/prek-armv7-unknown-linux-musleabihf.tar.gz.sha256) |

#### Verifying GitHub Artifact Attestations

The artifacts in this release have attestations generated with GitHub Artifact Attestations. These can be verified by using the [GitHub CLI](https://cli.github.com/manual/gh_attestation_verify):

```sh
gh attestation verify <file-path of downloaded artifact> --repo j178/prek
```

You can also download the attestation from [GitHub](https://redirect.github.com/j178/prek/attestations) and verify against that directly:

```sh
gh attestation verify <file-path of downloaded artifact> --bundle <file-path of downloaded attestation>
```

### [`v0.4.12`](https://redirect.github.com/j178/prek/releases/tag/v0.4.12): 0.4.12

[Compare Source](https://redirect.github.com/j178/prek/compare/v0.4.11...v0.4.12)

#### Release Notes

Released on 2026-08-03.

##### Enhancements

- Add `--require-group` for hook group intersections ()
- Align fast-path and builtin pre-commit hooks ()
- Do not shuffle file list for verbose output ()
- Improve top-level command descriptions ()
- Install `uv` from Astral CDN and drop source racing ()
- Make `prek install --force` bypass external hooks paths ()
- Show builtin hook flags in verbose list output ()
- Verify uv release archive checksums ()

##### Performance

- Precompute file tags in parallel ()
- Skip diffs after known hook modifications ()
- Skip worktree diffs for read-only languages ()
- Track builtin hook file changes directly ()

##### Bug fixes

- Use full object IDs in diff snapshots ()

##### Documentation

- Add a multi-repository configuration example ()
- Rewrite benchmark documentation ()

##### Contributors

- [@&#8203;j178](https://redirect.github.com/j178)
- [@&#8203;BitWeaverDev](https://redirect.github.com/BitWeaverDev)
- [@&#8203;allanlewis](https://redirect.github.com/allanlewis)

#### Install prek 0.4.12

##### Install prebuilt binaries via shell script

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/j178/prek/releases/download/v0.4.12/prek-installer.sh | sh
```

##### Install prebuilt binaries via powershell script

```sh
powershell -ExecutionPolicy Bypass -c "irm https://github.com/j178/prek/releases/download/v0.4.12/prek-installer.ps1 | iex"
```

##### Install prebuilt binaries via Homebrew

```sh
brew install prek
```

#### Download prek 0.4.12

| File                                                                                                                                            | Platform                     | Checksum                                                                                                             |
| ----------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| [prek-aarch64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-aarch64-apple-darwin.tar.gz)                     | Apple Silicon macOS          | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-aarch64-apple-darwin.tar.gz.sha256)           |
| [prek-x86\_64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-x86_64-apple-darwin.tar.gz)                      | Intel macOS                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-x86_64-apple-darwin.tar.gz.sha256)            |
| [prek-aarch64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-aarch64-pc-windows-msvc.zip)                     | ARM64 Windows                | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-aarch64-pc-windows-msvc.zip.sha256)           |
| [prek-i686-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-i686-pc-windows-msvc.zip)                           | x86 Windows                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-i686-pc-windows-msvc.zip.sha256)              |
| [prek-x86\_64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-x86_64-pc-windows-msvc.zip)                      | x64 Windows                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-x86_64-pc-windows-msvc.zip.sha256)            |
| [prek-aarch64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-aarch64-unknown-linux-gnu.tar.gz)           | ARM64 Linux                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-aarch64-unknown-linux-gnu.tar.gz.sha256)      |
| [prek-i686-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-i686-unknown-linux-gnu.tar.gz)                 | x86 Linux                    | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-i686-unknown-linux-gnu.tar.gz.sha256)         |
| [prek-riscv64gc-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-riscv64gc-unknown-linux-gnu.tar.gz)       | RISCV Linux                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-riscv64gc-unknown-linux-gnu.tar.gz.sha256)    |
| [prek-s390x-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-s390x-unknown-linux-gnu.tar.gz)               | S390x Linux                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-s390x-unknown-linux-gnu.tar.gz.sha256)        |
| [prek-x86\_64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-x86_64-unknown-linux-gnu.tar.gz)            | x64 Linux                    | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-x86_64-unknown-linux-gnu.tar.gz.sha256)       |
| [prek-armv7-unknown-linux-gnueabihf.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-armv7-unknown-linux-gnueabihf.tar.gz)   | ARMv7 Linux                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-armv7-unknown-linux-gnueabihf.tar.gz.sha256)  |
| [prek-aarch64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-aarch64-unknown-linux-musl.tar.gz)         | ARM64 MUSL Linux             | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-aarch64-unknown-linux-musl.tar.gz.sha256)     |
| [prek-i686-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-i686-unknown-linux-musl.tar.gz)               | x86 MUSL Linux               | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-i686-unknown-linux-musl.tar.gz.sha256)        |
| [prek-x86\_64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-x86_64-unknown-linux-musl.tar.gz)          | x64 MUSL Linux               | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-x86_64-unknown-linux-musl.tar.gz.sha256)      |
| [prek-arm-unknown-linux-musleabihf.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-arm-unknown-linux-musleabihf.tar.gz)     | ARMv6 MUSL Linux (Hardfloat) | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-arm-unknown-linux-musleabihf.tar.gz.sha256)   |
| [prek-armv7-unknown-linux-musleabihf.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-armv7-unknown-linux-musleabihf.tar.gz) | ARMv7 MUSL Linux             | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.12/prek-armv7-unknown-linux-musleabihf.tar.gz.sha256) |

#### Verifying GitHub Artifact Attestations

The artifacts in this release have attestations generated with GitHub Artifact Attestations. These can be verified by using the [GitHub CLI](https://cli.github.com/manual/gh_attestation_verify):

```sh
gh attestation verify <file-path of downloaded artifact> --repo j178/prek
```

You can also download the attestation from [GitHub](https://redirect.github.com/j178/prek/attestations) and verify against that directly:

```sh
gh attestation verify <file-path of downloaded artifact> --bundle <file-path of downloaded attestation>
```

### [`v0.4.11`](https://redirect.github.com/j178/prek/releases/tag/v0.4.11): 0.4.11

[Compare Source](https://redirect.github.com/j178/prek/compare/v0.4.10...v0.4.11)

#### Release Notes

Released on 2026-07-25.

##### Highlights

- This release adds two new builtin hooks, `deny-pattern` and `require-pattern`,
  as native alternatives for `pygrep` use cases. `deny-pattern` fails when a
  configured pattern is found, while `require-pattern` ensures every selected
  file contains a match. By matching natively without spawning a Python
  subprocess, they run over 4x faster than `pygrep` in benchmarks. Note that
  they use [Rust `regex` syntax](https://docs.rs/regex/latest/regex/#syntax), which does
  not support look-around features such as negative lookbehind.

- `prek run` now supports `--glob <PATTERN>` to run hooks on tracked files
  matching a glob. It can be repeated or combined with `--files` and
  `--directory`.

- Hook priorities now support reusable aliases:

  ```toml
  [priorities]
  checks = 10

  [[repos]]
  repo = "builtin"
  hooks = [
    { id = "check-json", priority = "checks" },
    { id = "check-yaml", priority = "checks" },
  ]
  ```

  This makes parallel scheduling easier to read and maintain.

##### Enhancements

- Add `deny-pattern` and `require-pattern` builtin hooks ()
- Support `--glob` patterns in `prek run` ()
- Support reusable aliases for hook priorities ()
- Implement `requirements-txt-fixer` as a builtin hook ()
- Improve user-facing warnings and errors ()
- Install Node hooks through git url ()

##### Performance

- Reduce blocking-pool overhead in file hooks ()
- Speed up mixed-line-ending scans with memchr2 ()

##### Bug fixes

- Honor filenames in builtin hook entry and args ()
- Match identify tags across filename parts ()
- Preserve hook output order with a shared pipe ()
- Preserve system download policy when applying metadata ()

##### Contributors

- [@&#8203;j178](https://redirect.github.com/j178)
- [@&#8203;chrisoro](https://redirect.github.com/chrisoro)

#### Install prek 0.4.11

##### Install prebuilt binaries via shell script

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/j178/prek/releases/download/v0.4.11/prek-installer.sh | sh
```

##### Install prebuilt binaries via powershell script

```sh
powershell -ExecutionPolicy Bypass -c "irm https://github.com/j178/prek/releases/download/v0.4.11/prek-installer.ps1 | iex"
```

##### Install prebuilt binaries via Homebrew

```sh
brew install prek
```

#### Download prek 0.4.11

| File                                                                                                                                            | Platform                     | Checksum                                                                                                             |
| ----------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| [prek-aarch64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.11/prek-aarch64-apple-darwin.tar.gz)                     | Apple Silicon macOS          | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.11/prek-aarch64-apple-darwin.tar.gz.sha256)           |
| [prek-x86\_64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.4.11/prek-x86_64-apple-darwin.tar.gz)                      | Intel macOS                  | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.11/prek-x86_64-apple-darwin.tar.gz.sha256)            |
| [prek-aarch64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.4.11/prek-aarch64-pc-windows-msvc.zip)                     | ARM64 Windows                | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.4.11/prek-aarch64-pc-windows-msvc.zip.sha256)           |
| [prek-i686-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.4.11/prek-i686-pc-

> ❗ **Important**
>
> ✂ PR body was truncated to here.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b7818fb97184`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
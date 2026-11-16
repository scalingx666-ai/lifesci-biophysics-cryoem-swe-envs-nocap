# Issue

**Title:** Update dependency prek to v0.5.10

## Description

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [prek](https://redirect.github.com/j178/prek) | uses-with | patch | `v0.5.7` → `v0.5.10` |

---

### Release Notes

<details>
<summary>j178/prek (prek)</summary>

### [`v0.5.10`](https://redirect.github.com/j178/prek/releases/tag/v0.5.10): 0.5.10

[Compare Source](https://redirect.github.com/j178/prek/compare/v0.5.9...v0.5.10)

##### Release Notes

Released on 2026-11-04.

##### Enhancements

- Add a builtin `replace-pattern` hook ()

##### Bug fixes

- Avoid downloading Node prereleases by default ()
- Keep file checks out of rayon's recursive frames ()
- Skip Node releases without a compatible archive ()

##### Documentation

- Add equivalent prek.toml examples throughout the documentation ()

##### Contributors

- [@&#8203;connorbrinton](https://redirect.github.com/connorbrinton)
- [@&#8203;j178](https://redirect.github.com/j178)

##### Install prek 0.5.10

##### Install prebuilt binaries via shell script

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/j178/prek/releases/download/v0.5.10/prek-installer.sh | sh
```

##### Install prebuilt binaries via powershell script

```sh
powershell -ExecutionPolicy Bypass -c "irm https://github.com/j178/prek/releases/download/v0.5.10/prek-installer.ps1 | iex"
```

##### Install prebuilt binaries via Homebrew

```sh
brew install prek
```

##### Download prek 0.5.10

| File                                                                                                                                          | Platform            | Checksum                                                                                                            |
| --------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------- |
| [prek-aarch64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.10/prek-aarch64-apple-darwin.tar.gz)                   | Apple Silicon macOS | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.10/prek-aarch64-apple-darwin.tar.gz.sha256)          |
| [prek-x86_64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.10/prek-x86_64-apple-darwin.tar.gz)                     | Intel macOS         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.10/prek-x86_64-apple-darwin.tar.gz.sha256)           |
| [prek-aarch64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.5.10/prek-aarch64-pc-windows-msvc.zip)                   | ARM64 Windows       | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.10/prek-aarch64-pc-windows-msvc.zip.sha256)          |
| [prek-x86_64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.5.10/prek-x86_64-pc-windows-msvc.zip)                     | x64 Windows         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.10/prek-x86_64-pc-windows-msvc.zip.sha256)           |
| [prek-aarch64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.10/prek-aarch64-unknown-linux-gnu.tar.gz)         | ARM64 Linux         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.10/prek-aarch64-unknown-linux-gnu.tar.gz.sha256)     |
| [prek-powerpc64le-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.10/prek-powerpc64le-unknown-linux-gnu.tar.gz) | PPC64LE Linux       | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.10/prek-powerpc64le-unknown-linux-gnu.tar.gz.sha256) |
| [prek-x86_64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.10/prek-x86_64-unknown-linux-gnu.tar.gz)           | x64 Linux           | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.10/prek-x86_64-unknown-linux-gnu.tar.gz.sha256)      |
| [prek-aarch64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.10/prek-aarch64-unknown-linux-musl.tar.gz)       | ARM64 MUSL Linux    | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.10/prek-aarch64-unknown-linux-musl.tar.gz.sha256)    |
| [prek-x86_64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.10/prek-x86_64-unknown-linux-musl.tar.gz)         | x64 MUSL Linux      | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.10/prek-x86_64-unknown-linux-musl.tar.gz.sha256)     |

##### Verifying GitHub Artifact Attestations

The artifacts in this release have attestations generated with GitHub Artifact Attestations. These can be verified by using the [GitHub CLI](https://cli.github.com/manual/gh_attestation_verify):

```sh
gh attestation verify <file-path of downloaded artifact> --repo j178/prek
```

You can also download the attestation from [GitHub](https://redirect.github.com/j178/prek/attestations) and verify against that directly:

```sh
gh attestation verify <file-path of downloaded artifact> --bundle <file-path of downloaded attestation>
```

### [`v0.5.9`](https://redirect.github.com/j178/prek/releases/tag/v0.5.9): 0.5.9

[Compare Source](https://redirect.github.com/j178/prek/compare/v0.5.7...v0.5.9)

#### Release Notes

Released on 2026-10-30.

##### Enhancements

- Add `normalize-indentation` builtin hook ()
- Allow overriding the uv install version ()
- Embed dependency metadata in release binaries ()
- Select musl Bun builds when glibc is unavailable ()

##### Performance

- Improve workspace discovery performance ()

##### Bug fixes

- Preserve clean-filtered files during stashing ()

##### Contributors

- [@&#8203;j178](https://redirect.github.com/j178)
- [@&#8203;tchernobog](https://redirect.github.com/tchernobog)

#### Install prek 0.5.9

##### Install prebuilt binaries via shell script

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/j178/prek/releases/download/v0.5.9/prek-installer.sh | sh
```

##### Install prebuilt binaries via powershell script

```sh
powershell -ExecutionPolicy Bypass -c "irm https://github.com/j178/prek/releases/download/v0.5.9/prek-installer.ps1 | iex"
```

##### Install prebuilt binaries via Homebrew

```sh
brew install prek
```

#### Download prek 0.5.9

| File                                                                                                                                         | Platform            | Checksum                                                                                                           |
| -------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------ |
| [prek-aarch64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.9/prek-aarch64-apple-darwin.tar.gz)                   | Apple Silicon macOS | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.9/prek-aarch64-apple-darwin.tar.gz.sha256)          |
| [prek-x86_64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.9/prek-x86_64-apple-darwin.tar.gz)                     | Intel macOS         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.9/prek-x86_64-apple-darwin.tar.gz.sha256)           |
| [prek-aarch64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.5.9/prek-aarch64-pc-windows-msvc.zip)                   | ARM64 Windows       | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.9/prek-aarch64-pc-windows-msvc.zip.sha256)          |
| [prek-x86_64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.5.9/prek-x86_64-pc-windows-msvc.zip)                     | x64 Windows         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.9/prek-x86_64-pc-windows-msvc.zip.sha256)           |
| [prek-aarch64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.9/prek-aarch64-unknown-linux-gnu.tar.gz)         | ARM64 Linux         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.9/prek-aarch64-unknown-linux-gnu.tar.gz.sha256)     |
| [prek-powerpc64le-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.9/prek-powerpc64le-unknown-linux-gnu.tar.gz) | PPC64LE Linux       | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.9/prek-powerpc64le-unknown-linux-gnu.tar.gz.sha256) |
| [prek-x86_64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.9/prek-x86_64-unknown-linux-gnu.tar.gz)           | x64 Linux           | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.9/prek-x86_64-unknown-linux-gnu.tar.gz.sha256)      |
| [prek-aarch64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.9/prek-aarch64-unknown-linux-musl.tar.gz)       | ARM64 MUSL Linux    | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.9/prek-aarch64-unknown-linux-musl.tar.gz.sha256)    |
| [prek-x86_64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.9/prek-x86_64-unknown-linux-musl.tar.gz)         | x64 MUSL Linux      | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.9/prek-x86_64-unknown-linux-musl.tar.gz.sha256)     |

#### Verifying GitHub Artifact Attestations

The artifacts in this release have attestations generated with GitHub Artifact Attestations. These can be verified by using the [GitHub CLI](https://cli.github.com/manual/gh_attestation_verify):

```sh
gh attestation verify <file-path of downloaded artifact> --repo j178/prek
```

You can also download the attestation from [GitHub](https://redirect.github.com/j178/prek/attestations) and verify against that directly:

```sh
gh attestation verify <file-path of downloaded artifact> --bundle <file-path of downloaded attestation>
```

</details>

---

### Configuration

📅 **Schedule**: (in timezone America/Detroit)

- Branch creation
  - Between 05:00 PM and 11:59 PM, only on Wednesday (`* 17-23 * * 3`)
- Automerge
  - At any time (no schedule defined)

🚦 **Automerge**: Enabled.

♻ **Rebasing**: Whenever PR is behind base branch, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/glotzerlab/freud).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0NC4yMDQuMCIsInVwZGF0ZWRJblZlciI6IjQ0LjIwNC4wIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJkZXBlbmRlbmNpZXMiXX0=-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `25dddec0113f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
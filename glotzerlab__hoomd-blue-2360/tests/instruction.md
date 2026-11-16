# Issue

**Title:** Update dependency prek to v0.5.6

## Description

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [prek](https://redirect.github.com/j178/prek) | uses-with | patch | `v0.5.5` → `v0.5.6` |

---

### Release Notes

<details>
<summary>j178/prek (prek)</summary>

### [`v0.5.6`](https://redirect.github.com/j178/prek/releases/tag/v0.5.6): 0.5.6

[Compare Source](https://redirect.github.com/j178/prek/compare/v0.5.5...v0.5.6)

#### Release Notes

Released on 2026-10-19.

##### Enhancements

- Add builtin hook `check-commit-trailers` ()
- Add builtin hook `check-signed-commit` ()

##### Performance

- Enable PGO for native release builds ()
- Reduce hook process launches with balanced batches ()
- Resolve hook commands before batching ()

##### Bug fixes

- Accept UTF-8 BOM in `check-json` ()
- Preserve unquoted dotted revisions in `prek update` ()

##### Documentation

- Document standalone installer options ()
- Update benchmarks for prek 0.5.5 ()

##### Other changes

- Enable ICF for release builds ()
- Update uv to 0.13.0 ()

##### Contributors

- [@&#8203;ebuildy](https://redirect.github.com/ebuildy)
- [@&#8203;j178](https://redirect.github.com/j178)
- [@&#8203;shaanmajid](https://redirect.github.com/shaanmajid)

#### Install prek 0.5.6

##### Install prebuilt binaries via shell script

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/j178/prek/releases/download/v0.5.6/prek-installer.sh | sh
```

##### Install prebuilt binaries via powershell script

```sh
powershell -ExecutionPolicy Bypass -c "irm https://github.com/j178/prek/releases/download/v0.5.6/prek-installer.ps1 | iex"
```

##### Install prebuilt binaries via Homebrew

```sh
brew install prek
```

#### Download prek 0.5.6

| File                                                                                                                                         | Platform            | Checksum                                                                                                           |
| -------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------ |
| [prek-aarch64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.6/prek-aarch64-apple-darwin.tar.gz)                   | Apple Silicon macOS | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.6/prek-aarch64-apple-darwin.tar.gz.sha256)          |
| [prek-x86_64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.6/prek-x86_64-apple-darwin.tar.gz)                     | Intel macOS         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.6/prek-x86_64-apple-darwin.tar.gz.sha256)           |
| [prek-aarch64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.5.6/prek-aarch64-pc-windows-msvc.zip)                   | ARM64 Windows       | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.6/prek-aarch64-pc-windows-msvc.zip.sha256)          |
| [prek-x86_64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.5.6/prek-x86_64-pc-windows-msvc.zip)                     | x64 Windows         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.6/prek-x86_64-pc-windows-msvc.zip.sha256)           |
| [prek-aarch64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.6/prek-aarch64-unknown-linux-gnu.tar.gz)         | ARM64 Linux         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.6/prek-aarch64-unknown-linux-gnu.tar.gz.sha256)     |
| [prek-powerpc64le-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.6/prek-powerpc64le-unknown-linux-gnu.tar.gz) | PPC64LE Linux       | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.6/prek-powerpc64le-unknown-linux-gnu.tar.gz.sha256) |
| [prek-x86_64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.6/prek-x86_64-unknown-linux-gnu.tar.gz)           | x64 Linux           | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.6/prek-x86_64-unknown-linux-gnu.tar.gz.sha256)      |
| [prek-aarch64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.6/prek-aarch64-unknown-linux-musl.tar.gz)       | ARM64 MUSL Linux    | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.6/prek-aarch64-unknown-linux-musl.tar.gz.sha256)    |
| [prek-x86_64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.6/prek-x86_64-unknown-linux-musl.tar.gz)         | x64 MUSL Linux      | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.6/prek-x86_64-unknown-linux-musl.tar.gz.sha256)     |

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
  - At any time (no schedule defined)
- Automerge
  - At any time (no schedule defined)

🚦 **Automerge**: Enabled.

♻ **Rebasing**: Whenever PR is behind base branch, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/glotzerlab/hoomd-blue).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0NC4xNjQuMSIsInVwZGF0ZWRJblZlciI6IjQ0LjE2NC4xIiwidGFyZ2V0QnJhbmNoIjoidHJ1bmsiLCJsYWJlbHMiOlsiZGVwZW5kZW5jaWVzIl19-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c87fc0885b09`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
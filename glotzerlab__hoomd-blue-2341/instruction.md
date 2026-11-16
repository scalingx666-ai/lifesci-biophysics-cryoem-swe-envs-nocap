# Issue

**Title:** Update dependency prek to v0.5.4

## Description

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [prek](https://redirect.github.com/j178/prek) | uses-with | patch | `v0.5.3` → `v0.5.4` |

---

### Release Notes

<details>
<summary>j178/prek (prek)</summary>

### [`v0.5.4`](https://redirect.github.com/j178/prek/releases/tag/v0.5.4): 0.5.4

[Compare Source](https://redirect.github.com/j178/prek/compare/v0.5.3...v0.5.4)

##### Release Notes

Released on 2026-09-28.

##### Highlights

##### Faster builtin hooks

In our end-to-end benchmark, prek is about 38% faster than 0.5.3, with some builtin
hooks up to 273% faster (`check-yaml`: 273%, `check-json`: 80%,
`check-merge-conflict`: 71%).

##### Enhancements

- Add `include_deleted` to allow hooks to include deleted files ()
- Add `--check` and simplify `end-of-file-fixer` ()
- Add `--check` to `file-contents-sorter` ()
- Add `--check` to `requirements-txt-fixer` ()
- Add `--check` to `trailing-whitespace` ()
- Expose and document `prek util generate-shell-completion` ()
- Support `hide_status` in project and user configuration ()
- Support look-around regex in builtin pattern hooks ()

##### Performance

- Cache the resolved Git executable on macOS ()
- Combine and cache Git repository path queries ()
- Optimize common builtin hook execution ()
- Optimize scanning in `mixed-line-ending` and `trailing-whitespace` ()
- Scan `check-merge-conflict` files in fixed-size blocks ()
- Use SIMD UTF-8 validation in `check-json`, `check-toml`, and `check-yaml` ()

##### Bug fixes

- Retry transient rename failures on Windows ()
- Use PATH to resolve prek in completion scripts ()

##### Documentation

- Clarify the flow and scope of usage guides ()
- Reorganize usage guides and reference documentation ()

##### Other changes

- Sync latest identify tags ()

##### Contributors

- [@&#8203;github-actions](https://redirect.github.com/github-actions)
- [@&#8203;j178](https://redirect.github.com/j178)

##### Install prek 0.5.4

##### Install prebuilt binaries via shell script

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/j178/prek/releases/download/v0.5.4/prek-installer.sh | sh
```

##### Install prebuilt binaries via powershell script

```sh
powershell -ExecutionPolicy Bypass -c "irm https://github.com/j178/prek/releases/download/v0.5.4/prek-installer.ps1 | iex"
```

##### Install prebuilt binaries via Homebrew

```sh
brew install prek
```

##### Download prek 0.5.4

| File                                                                                                                                   | Platform            | Checksum                                                                                                        |
| -------------------------------------------------------------------------------------------------------------------------------------- | ------------------- | --------------------------------------------------------------------------------------------------------------- |
| [prek-aarch64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.4/prek-aarch64-apple-darwin.tar.gz)             | Apple Silicon macOS | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.4/prek-aarch64-apple-darwin.tar.gz.sha256)       |
| [prek-x86\_64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.4/prek-x86_64-apple-darwin.tar.gz)              | Intel macOS         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.4/prek-x86_64-apple-darwin.tar.gz.sha256)        |
| [prek-aarch64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.5.4/prek-aarch64-pc-windows-msvc.zip)             | ARM64 Windows       | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.4/prek-aarch64-pc-windows-msvc.zip.sha256)       |
| [prek-x86\_64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.5.4/prek-x86_64-pc-windows-msvc.zip)              | x64 Windows         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.4/prek-x86_64-pc-windows-msvc.zip.sha256)        |
| [prek-aarch64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.4/prek-aarch64-unknown-linux-gnu.tar.gz)   | ARM64 Linux         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.4/prek-aarch64-unknown-linux-gnu.tar.gz.sha256)  |
| [prek-x86\_64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.4/prek-x86_64-unknown-linux-gnu.tar.gz)    | x64 Linux           | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.4/prek-x86_64-unknown-linux-gnu.tar.gz.sha256)   |
| [prek-aarch64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.4/prek-aarch64-unknown-linux-musl.tar.gz) | ARM64 MUSL Linux    | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.4/prek-aarch64-unknown-linux-musl.tar.gz.sha256) |
| [prek-x86\_64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.4/prek-x86_64-unknown-linux-musl.tar.gz)  | x64 MUSL Linux      | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.4/prek-x86_64-unknown-linux-musl.tar.gz.sha256)  |

##### Verifying GitHub Artifact Attestations

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0NC4xMTIuMCIsInVwZGF0ZWRJblZlciI6IjQ0LjExMi4wIiwidGFyZ2V0QnJhbmNoIjoidHJ1bmsiLCJsYWJlbHMiOlsiZGVwZW5kZW5jaWVzIl19-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f18930e7f85c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
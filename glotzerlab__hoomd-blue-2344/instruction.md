# Issue

**Title:** Update dependency prek to v0.5.5

## Description

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [prek](https://redirect.github.com/j178/prek) | uses-with | patch | `v0.5.4` → `v0.5.5` |

---

### Release Notes

<details>
<summary>j178/prek (prek)</summary>

### [`v0.5.5`](https://redirect.github.com/j178/prek/releases/tag/v0.5.5): 0.5.5

[Compare Source](https://redirect.github.com/j178/prek/compare/v0.5.4...v0.5.5)

#### Release Notes

Released on 2026-10-05.

##### Enhancements

- Add Nushell completion generation ()
- Add `ppc64le` wheel and binary builds ()

##### Performance

- Avoid JSON imports when querying Python interpreters ()
- Avoid duplicate diffs after hooks rewrite files ()
- Batch file reads in shebang checks ()
- Check cached hook environments concurrently ()
- Overlap Git discovery with worktree status checks ()
- Query staged additions only for case conflicts ()
- Reduce worker overhead in XML and pygrep checks ()
- Reuse Git worktree state during hook startup ()
- Reuse intent-to-add paths when saving unstaged changes ()
- Run Git queries concurrently for case-conflict checks ()
- Share Bun and Deno version queries across environments ()
- Share Lua runtime probes across hook environments ()
- Share Ruby version queries across hook environments ()
- Share mise version queries across hook environments ()
- Skip site initialization in Python interpreter probes ()
- Skip untracked files in destroyed symlink checks ()
- Skip unused file input for always-run hooks ()

##### Bug fixes

- Preserve staged and unstaged changes on conflicted stash restore ()
- Default missing `core.fileMode` in shebang checks ()
- Match protected branch patterns from the start ()
- Normalize explicit paths in shebang index checks ()
- Preserve empty `trailing-whitespace` character sets ()
- Reject empty files in `check-json` ()
- Reject trailing data in `check-json` ()
- Round up file sizes in `check-added-large-files` ()

##### Documentation

- Redesign the prek badge and refresh logo details ()

##### Contributors

- [@&#8203;imphil](https://redirect.github.com/imphil)
- [@&#8203;j178](https://redirect.github.com/j178)

#### Install prek 0.5.5

##### Install prebuilt binaries via shell script

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/j178/prek/releases/download/v0.5.5/prek-installer.sh | sh
```

##### Install prebuilt binaries via powershell script

```sh
powershell -ExecutionPolicy Bypass -c "irm https://github.com/j178/prek/releases/download/v0.5.5/prek-installer.ps1 | iex"
```

##### Install prebuilt binaries via Homebrew

```sh
brew install prek
```

#### Download prek 0.5.5

| File                                                                                                                                         | Platform            | Checksum                                                                                                           |
| -------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------ |
| [prek-aarch64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.5/prek-aarch64-apple-darwin.tar.gz)                   | Apple Silicon macOS | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.5/prek-aarch64-apple-darwin.tar.gz.sha256)          |
| [prek-x86\_64-apple-darwin.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.5/prek-x86_64-apple-darwin.tar.gz)                    | Intel macOS         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.5/prek-x86_64-apple-darwin.tar.gz.sha256)           |
| [prek-aarch64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.5.5/prek-aarch64-pc-windows-msvc.zip)                   | ARM64 Windows       | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.5/prek-aarch64-pc-windows-msvc.zip.sha256)          |
| [prek-x86\_64-pc-windows-msvc.zip](https://redirect.github.com/j178/prek/releases/download/v0.5.5/prek-x86_64-pc-windows-msvc.zip)                    | x64 Windows         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.5/prek-x86_64-pc-windows-msvc.zip.sha256)           |
| [prek-aarch64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.5/prek-aarch64-unknown-linux-gnu.tar.gz)         | ARM64 Linux         | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.5/prek-aarch64-unknown-linux-gnu.tar.gz.sha256)     |
| [prek-powerpc64le-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.5/prek-powerpc64le-unknown-linux-gnu.tar.gz) | PPC64LE Linux       | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.5/prek-powerpc64le-unknown-linux-gnu.tar.gz.sha256) |
| [prek-x86\_64-unknown-linux-gnu.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.5/prek-x86_64-unknown-linux-gnu.tar.gz)          | x64 Linux           | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.5/prek-x86_64-unknown-linux-gnu.tar.gz.sha256)      |
| [prek-aarch64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.5/prek-aarch64-unknown-linux-musl.tar.gz)       | ARM64 MUSL Linux    | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.5/prek-aarch64-unknown-linux-musl.tar.gz.sha256)    |
| [prek-x86\_64-unknown-linux-musl.tar.gz](https://redirect.github.com/j178/prek/releases/download/v0.5.5/prek-x86_64-unknown-linux-musl.tar.gz)        | x64 MUSL Linux      | [checksum](https://redirect.github.com/j178/prek/releases/download/v0.5.5/prek-x86_64-unknown-linux-musl.tar.gz.sha256)     |

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0NC4xMjUuMSIsInVwZGF0ZWRJblZlciI6IjQ0LjEyNS4xIiwidGFyZ2V0QnJhbmNoIjoidHJ1bmsiLCJsYWJlbHMiOlsiZGVwZW5kZW5jaWVzIl19-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b68c739e68f7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Update dependency prefix-dev/pixi to v0.82.0

## Description

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [prefix-dev/pixi](https://redirect.github.com/prefix-dev/pixi) | uses-with | minor | `v0.81.0` → `v0.82.0` |

---

### Release Notes

<details>
<summary>prefix-dev/pixi</summary>

### [`v0.82.0`](https://redirect.github.com/prefix-dev/pixi/releases/tag/v0.82.0)

[Compare Source](https://redirect.github.com/prefix-dev/pixi/compare/v0.81.0...v0.82.0)

##### \[0.82.0] - 2026-10-13

##### ✨ Highlights

Rich platforms now support AMD GPUs with `amdgpu = true` to declare GPU presence or `amdgpu = "gfx90a"` to require a specific GPU architecture.

`pixi global` now supports `exclude-newer` in the global manifest. Set a cutoff date or a duration such as `exclude-newer = "7d"` to exclude recently published packages from all global environments.

##### Added

- Add `exclude-newer` to the global manifest by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Use toml edit to modify config by [@&#8203;samrosenf](https://redirect.github.com/samrosenf) in
- Automake and autoconf for ros2_canopen by [@&#8203;Timple](https://redirect.github.com/Timple) in
- Support `PIXI_WORKSPACE_` env vars, make `PIXI_PROJECT_` env vars legacy by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- Add anonymous installation ping to install scripts by [@&#8203;wolfv](https://redirect.github.com/wolfv) in
- Add workload-identity login by [@&#8203;pavelzw](https://redirect.github.com/pavelzw) in
- Add a ratchet for `into_diagnostic` calls per crate by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- Add lucas as project co-author by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- Add recommended R packages as dependencies by [@&#8203;roaldarbol](https://redirect.github.com/roaldarbol) in
- Add Rust when SystemRequirements asks for Cargo by [@&#8203;roaldarbol](https://redirect.github.com/roaldarbol) in
- Support `amdgpu` in the rich platform table by [@&#8203;baszalmstra](https://redirect.github.com/baszalmstra) in
- Request the Basilisk audience for prefix.dev login by [@&#8203;nichmor](https://redirect.github.com/nichmor) in
- Automatically try increasing rlimit by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- Add version exemptions for exclude-newer by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in

##### Changed

- Pin mojo version to 1.0.0 in conda-script example by [@&#8203;aoright](https://redirect.github.com/aoright) in
- Bump quinn-proto to 0.11.15 by [@&#8203;wolfv](https://redirect.github.com/wolfv) in
- Reuse immutable Git build artifacts without checkout by [@&#8203;baszalmstra](https://redirect.github.com/baszalmstra) in
- Append newline to `.gitignore` if missing by [@&#8203;ravindra-RKB](https://redirect.github.com/ravindra-RKB) in
- Don't suggest a one-character alias for an unrelated subcommand by [@&#8203;Atishyy27](https://redirect.github.com/Atishyy27) in
- Read `package.xml` expressions with `and` and `or` by [@&#8203;ruben-arts](https://redirect.github.com/ruben-arts) in
- Replace `miette::Report` with typed errors by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- Carry typed names in manifest edit errors by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- Type the public `pixi_config` error signatures by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- Type the locked git url errors by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- Install into R's library on Windows by [@&#8203;roaldarbol](https://redirect.github.com/roaldarbol) in
- Type the project-defined mapping errors by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- Type the HTTP client, activation and env file errors by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- Keep the namespace in prefix.dev channel URLs by [@&#8203;alejandrofontan](https://redirect.github.com/alejandrofontan) in
- Type the resolution context errors by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in

##### Documentation

- Fix snippet include syntax for Python conda-script tutorial by [@&#8203;aoright](https://redirect.github.com/aoright) in
- Use `backend.channels` instead of the deprecated `package.build.channels` by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Mention bioconductor limitation by [@&#8203;jpintar](https://redirect.github.com/jpintar) in
- Add line break after overview header in README by [@&#8203;DARPrA2908](https://redirect.github.com/DARPrA2908) in
- Update social links in zensical.toml by [@&#8203;DARPrA2908](https://redirect.github.com/DARPrA2908) in
- Fix `Platform` -> `Subdir` link by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- Update direnv documentation by [@&#8203;pavelzw](https://redirect.github.com/pavelzw) in
- Document auth-logout input of setup-pixi v0.11.0 by [@&#8203;pavelzw](https://redirect.github.com/pavelzw) in

##### Fixed

- Concurrency config set correctly targets concurrency config by [@&#8203;aoright](https://redirect.github.com/aoright) in
- Sort source record dependencies and constraints for lockfile determinism by [@&#8203;aoright](https://redirect.github.com/aoright) in
- Update git dependencies sharing same repository and reference together by [@&#8203;aoright](https://redirect.github.com/aoright) in
- Allow removing current platform in pixi workspace platform remove by [@&#8203;aoright](https://redirect.github.com/aoright) in
- Accept custom per-package index in satisfiability for unindexed requirements by [@&#8203;aoright](https://redirect.github.com/aoright) in
- Preserve error chain and diagnostics on build dispatch failure by [@&#8203;aoright](https://redirect.github.com/aoright) in
- Pass stored credentials to the OCI middleware by [@&#8203;chris-thomas-dev](https://redirect.github.com/chris-thomas-dev) in
- Preserve quotes in task arguments by [@&#8203;tdejager](https://redirect.github.com/tdejager) in
- CLI arg `--clean-env` is no longer ignored by [@&#8203;tdejager](https://redirect.github.com/tdejager) in
- Support all config keys in `pixi config list <key>` by [@&#8203;tdejager](https://redirect.github.com/tdejager) in
- Honor CARGO_BUILD_RUSTFLAGS in PGO builds by [@&#8203;baszalmstra](https://redirect.github.com/baszalmstra) in
- Invalidate source build caches on model and config changes by [@&#8203;tdejager](https://redirect.github.com/tdejager) in
- Fix missing `setuptools` in ros test by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- Fingerprint packages without SHA-256 checksums by [@&#8203;tdejager](https://redirect.github.com/tdejager) in
- Fix `insta-review` task by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in

##### Performance

- Build Linux x64 releases with PGO by [@&#8203;baszalmstra](https://redirect.github.com/baszalmstra) in
- Build macOS ARM64 releases with PGO by [@&#8203;baszalmstra](https://redirect.github.com/baszalmstra) in
- Build Windows x64 releases with PGO by [@&#8203;baszalmstra](https://redirect.github.com/baszalmstra) in

##### Refactor

- Type the marker environment and backend override errors by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in

##### Removed

- Remove old aws-lc-sys cmake workaround by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- Remove xfail for multi-output multi-executable test by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in

##### New Contributors

- [@&#8203;alejandrofontan](https://redirect.github.com/alejandrofontan) made their first contribution in
- [@&#8203;Atishyy27](https://redirect.github.com/Atishyy27) made their first contribution in
- [@&#8203;ravindra-RKB](https://redirect.github.com/ravindra-RKB) made their first contribution in
- [@&#8203;00200200](https://redirect.github.com/00200200) made their first contribution in
- [@&#8203;DARPrA2908](https://redirect.github.com/DARPrA2908) made their first contribution in
- [@&#8203;jpintar](https://redirect.github.com/jpintar) made their first contribution in
- [@&#8203;chris-thomas-dev](https://redirect.github.com/chris-thomas-dev) made their first contribution in
- [@&#8203;aoright](https://redirect.github.com/aoright) made their first contribution in

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0NC4xNTYuMyIsInVwZGF0ZWRJblZlciI6IjQ0LjE1Ni4zIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJkZXBlbmRlbmNpZXMiXX0=-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6119dd273b1e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
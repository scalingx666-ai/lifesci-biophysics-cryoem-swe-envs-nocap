# Issue

**Title:** Bump the actions-version group with 8 updates

## Description

Bumps the actions-version group with 8 updates:

| Package | From | To |
| --- | --- | --- |
| [actions/checkout](https://github.com/actions/checkout) | `4.1.7` | `4.2.2` |
| [pypa/cibuildwheel](https://github.com/pypa/cibuildwheel) | `2.20.0` | `2.22.0` |
| [actions/upload-artifact](https://github.com/actions/upload-artifact) | `4.3.6` | `4.6.0` |
| [actions/setup-python](https://github.com/actions/setup-python) | `5.1.0` | `5.4.0` |
| [glotzerlab/workflows](https://github.com/glotzerlab/workflows) | `0.5.0` | `0.6.0` |
| [pypa/gh-action-pypi-publish](https://github.com/pypa/gh-action-pypi-publish) | `1.9.0` | `1.12.4` |
| [softprops/action-gh-release](https://github.com/softprops/action-gh-release) | `2.0.8` | `2.2.1` |
| [mamba-org/setup-micromamba](https://github.com/mamba-org/setup-micromamba) | `2.0.0` | `2.0.4` |

Updates `actions/checkout` from 4.1.7 to 4.2.2
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/actions/checkout/releases">actions/checkout's releases</a>.</em></p>
<blockquote>
<h2>v4.2.2</h2>
<h2>What's Changed</h2>
<ul>
<li><code>url-helper.ts</code> now leverages well-known environment variables by <a href="https://github.com/jww3"><code>@​jww3</code></a> in <a href="">actions/checkout#1941</a></li>
<li>Expand unit test coverage for <code>isGhes</code> by <a href="https://github.com/jww3"><code>@​jww3</code></a> in <a href="">actions/checkout#1946</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/checkout/compare/v4.2.1...v4.2.2">https://github.com/actions/checkout/compare/v4.2.1...v4.2.2</a></p>
<h2>v4.2.1</h2>
<h2>What's Changed</h2>
<ul>
<li>Check out other refs/* by commit if provided, fall back to ref by <a href="https://github.com/orhantoy"><code>@​orhantoy</code></a> in <a href="">actions/checkout#1924</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/Jcambass"><code>@​Jcambass</code></a> made their first contribution in <a href="">actions/checkout#1919</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/checkout/compare/v4.2.0...v4.2.1">https://github.com/actions/checkout/compare/v4.2.0...v4.2.1</a></p>
<h2>v4.2.0</h2>
<h2>What's Changed</h2>
<ul>
<li>Add Ref and Commit outputs by <a href="https://github.com/lucacome"><code>@​lucacome</code></a> in <a href="">actions/checkout#1180</a></li>
<li>Dependabot updates in <a href="">actions/checkout#1777</a> &amp; <a href="">actions/checkout#1872</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/yasonk"><code>@​yasonk</code></a> made their first contribution in <a href="">actions/checkout#1869</a></li>
<li><a href="https://github.com/lucacome"><code>@​lucacome</code></a> made their first contribution in <a href="">actions/checkout#1180</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/checkout/compare/v4.1.7...v4.2.0">https://github.com/actions/checkout/compare/v4.1.7...v4.2.0</a></p>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/actions/checkout/blob/main/CHANGELOG.md">actions/checkout's changelog</a>.</em></p>
<blockquote>
<h1>Changelog</h1>
<h2>v4.2.2</h2>
<ul>
<li><code>url-helper.ts</code> now leverages well-known environment variables by <a href="https://github.com/jww3"><code>@​jww3</code></a> in <a href="">actions/checkout#1941</a></li>
<li>Expand unit test coverage for <code>isGhes</code> by <a href="https://github.com/jww3"><code>@​jww3</code></a> in <a href="">actions/checkout#1946</a></li>
</ul>
<h2>v4.2.1</h2>
<ul>
<li>Check out other refs/* by commit if provided, fall back to ref by <a href="https://github.com/orhantoy"><code>@​orhantoy</code></a> in <a href="">actions/checkout#1924</a></li>
</ul>
<h2>v4.2.0</h2>
<ul>
<li>Add Ref and Commit outputs by <a href="https://github.com/lucacome"><code>@​lucacome</code></a> in <a href="">actions/checkout#1180</a></li>
<li>Dependency updates by <a href="https://github.com/dependabot"><code>@​dependabot</code></a>- <a href="">actions/checkout#1777</a>, <a href="">actions/checkout#1872</a></li>
</ul>
<h2>v4.1.7</h2>
<ul>
<li>Bump the minor-npm-dependencies group across 1 directory with 4 updates by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">actions/checkout#1739</a></li>
<li>Bump actions/checkout from 3 to 4 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">actions/checkout#1697</a></li>
<li>Check out other refs/* by commit by <a href="https://github.com/orhantoy"><code>@​orhantoy</code></a> in <a href="">actions/checkout#1774</a></li>
<li>Pin actions/checkout's own workflows to a known, good, stable version. by <a href="https://github.com/jww3"><code>@​jww3</code></a> in <a href="">actions/checkout#1776</a></li>
</ul>
<h2>v4.1.6</h2>
<ul>
<li>Check platform to set archive extension appropriately by <a href="https://github.com/cory-miller"><code>@​cory-miller</code></a> in <a href="">actions/checkout#1732</a></li>
</ul>
<h2>v4.1.5</h2>
<ul>
<li>Update NPM dependencies by <a href="https://github.com/cory-miller"><code>@​cory-miller</code></a> in <a href="">actions/checkout#1703</a></li>
<li>Bump github/codeql-action from 2 to 3 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">actions/checkout#1694</a></li>
<li>Bump actions/setup-node from 1 to 4 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">actions/checkout#1696</a></li>
<li>Bump actions/upload-artifact from 2 to 4 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">actions/checkout#1695</a></li>
<li>README: Suggest <code>user.email</code> to be <code>41898282+github-actions[bot]@users.noreply.github.com</code> by <a href="https://github.com/cory-miller"><code>@​cory-miller</code></a> in <a href="">actions/checkout#1707</a></li>
</ul>
<h2>v4.1.4</h2>
<ul>
<li>Disable <code>extensions.worktreeConfig</code> when disabling <code>sparse-checkout</code> by <a href="https://github.com/jww3"><code>@​jww3</code></a> in <a href="">actions/checkout#1692</a></li>
<li>Add dependabot config by <a href="https://github.com/cory-miller"><code>@​cory-miller</code></a> in <a href="">actions/checkout#1688</a></li>
<li>Bump the minor-actions-dependencies group with 2 updates by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">actions/checkout#1693</a></li>
<li>Bump word-wrap from 1.2.3 to 1.2.5 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">actions/checkout#1643</a></li>
</ul>
<h2>v4.1.3</h2>
<ul>
<li>Check git version before attempting to disable <code>sparse-checkout</code> by <a href="https://github.com/jww3"><code>@​jww3</code></a> in <a href="">actions/checkout#1656</a></li>
<li>Add SSH user parameter by <a href="https://github.com/cory-miller"><code>@​cory-miller</code></a> in <a href="">actions/checkout#1685</a></li>
<li>Update <code>actions/checkout</code> version in <code>update-main-version.yml</code> by <a href="https://github.com/jww3"><code>@​jww3</code></a> in <a href="">actions/checkout#1650</a></li>
</ul>
<h2>v4.1.2</h2>
<ul>
<li>Fix: Disable sparse checkout whenever <code>sparse-checkout</code> option is not present <a href="https://github.com/dscho"><code>@​dscho</code></a> in <a href="">actions/checkout#1598</a></li>
</ul>
<h2>v4.1.1</h2>
<ul>
<li>Correct link to GitHub Docs by <a href="https://github.com/peterbe"><code>@​peterbe</code></a> in <a href="">actions/checkout#1511</a></li>
<li>Link to release page from what's new section by <a href="https://github.com/cory-miller"><code>@​cory-miller</code></a> in <a href="">actions/checkout#1514</a></li>
</ul>
<h2>v4.1.0</h2>
<ul>
<li><a href="">Add support for partial checkout filters</a></li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>11bd719</code></a> Prepare 4.2.2 Release (<a href="">#1953</a>)</li>
<li><a href=""><code>e3d2460</code></a> Expand unit test coverage (<a href="">#1946</a>)</li>
<li><a href=""><code>163217d</code></a> <code>url-helper.ts</code> now leverages well-known environment variables. (<a href="">#1941</a>)</li>
<li><a href=""><code>eef6144</code></a> Prepare 4.2.1 release (<a href="">#1925</a>)</li>
<li><a href=""><code>6b42224</code></a> Add workflow file for publishing releases to immutable action package (<a href="">#1919</a>)</li>
<li><a href=""><code>de5a000</code></a> Check out other refs/* by commit if provided, fall back to ref (<a href="">#1924</a>)</li>
<li><a href=""><code>d632683</code></a> Prepare 4.2.0 release (<a href="">#1878</a>)</li>
<li><a href=""><code>6d193bf</code></a> Bump braces from 3.0.2 to 3.0.3 (<a href="">#1777</a>)</li>
<li><a href=""><code>db0cee9</code></a> Bump the minor-npm-dependencies group across 1 directory with 4 updates (<a href="">#1872</a>)</li>
<li><a href=""><code>b684943</code></a> Add Ref and Commit outputs (<a href="">#1180</a>)</li>
<li>Additional commits viewable in <a href="https://github.com/actions/checkout/compare/v4.1.7...11bd71901bbe5b1630ceea73d27597364c9af683">compare view</a></li>
</ul>
</details>
<br />

Updates `pypa/cibuildwheel` from 2.20.0 to 2.22.0
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/pypa/cibuildwheel/releases">pypa/cibuildwheel's releases</a>.</em></p>
<blockquote>
<h2>Version 2.22.0</h2>
<ul>
<li>🌟 Added a new <code>CIBW_ENABLE</code>/<code>enable</code> feature that replaces <code>CIBW_FREETHREADED_SUPPORT</code>/<code>free-threaded-support</code> and <code>CIBW_PRERELEASE_PYTHONS</code> with a system that supports both. In cibuildwheel 3, this will also include a PyPy setting and the deprecated options will be removed. (<a href="">#2048</a>)</li>
<li>🌟 <a href="https://peps.python.org/pep-0735/">Dependency groups</a> are now supported for tests. Use <code>CIBW_TEST_GROUPS</code>/<code>test-groups</code> to specify groups in <code>[dependency-groups]</code> for testing. (<a href="">#2063</a>)</li>
<li>🌟 Support for the experimental Ubuntu-based ARMv7l manylinux image (<a href="">#2052</a>)</li>
<li>✨ Show a warning when cibuildwheel is run from Python 3.10 or older; cibuildwheel 3.0 will require Python 3.11 or newer as host (<a href="">#2050</a>)</li>
<li>🐛 Fix issue with stderr interfering with checking the docker version (<a href="">#2074</a>)</li>
<li>🛠 Python 3.9 is now used in <code>CIBW_BEFORE_ALL</code>/<code>before-all</code> on linux, replacing 3.8, which is now EoL (<a href="">#2043</a>)</li>
<li>🛠 Error messages for producing a pure-Python wheel are slightly more informative (<a href="">#2044</a>)</li>
<li>🛠 Better error when <code>uname -m</code> fails on ARM (<a href="">#2049</a>)</li>
<li>🛠 Better error when repair fails and docs for abi3audit on Windows (<a href="">#2058</a>)</li>
<li>🛠 Better error when <code>manylinux-interpreters ensure</code> fails (<a href="">#2066</a>)</li>
<li>🛠 Update Pyodide to 0.26.4, and adapt to the unbundled pyodide-build (now 0.29) (<a href="">#2090</a>)</li>
<li>🛠 Now cibuildwheel uses dependency-groups for development dependencies (<a href="">#2064</a>, <a href="">#2085</a>)</li>
<li>📚 Docs updates and tidy ups (<a href="">#2061</a>, <a href="">#2067</a>, <a href="">#2072</a>)</li>
</ul>
<h2>v2.21.3</h2>
<ul>
<li>🛠 Update CPython 3.13 to 3.13.0 final release (<a href="">#2032</a>)</li>
<li>📚 Docs updates and tidy ups (<a href="">#2035</a>)</li>
</ul>
<h2>v2.21.2</h2>
<ul>
<li>✨ Adds support for building 32-bit armv7l wheels on musllinux. On a Linux system with emulation set up, set <a href="https://cibuildwheel.pypa.io/en/stable/options/#archs">CIBW_ARCHS</a> to <code>armv7l</code> to try it out if you're interested! (<a href="">#2017</a>)</li>
<li>🐛 Fix Linux Podman builds on some systems (<a href="">#2016</a>)</li>
<li>✨ Adds official support for running on Python 3.13 (<a href="">#2026</a>)</li>
<li>🛠 Update CPython 3.13 to 3.13.0rc3 (<a href="">#2029</a>)</li>
</ul>
<p>Note: the default <a href="https://cibuildwheel.pypa.io/en/stable/options/#linux-image">manylinux image</a> is <strong>scheduled to change</strong> from <code>manylinux2014</code> to <code>manylinux_2_28</code> in a cibuildwheel release on or after <strong>6th May 2025</strong> - you can set the value now to avoid getting upgraded if you want. (<a href="">#1992</a>)</p>
<h2>Version 2.21.1</h2>
<ul>
<li>🐛 Fix a bug in the Linux build, where files copied to the container would have invalid ownership permissions (<a href="">#2007</a>)</li>
<li>🐛 Fix a bug on Windows where cibuildwheel would call upon <code>uv</code> to install dependencies for versions of CPython that it does not support (<a href="">#2005</a>)</li>
<li>🐛 Fix a bug where <code>uv 0.4.10</code> would not use the right Python when testing on Linux. (<a href="">#2008</a>)</li>
<li>🛠 Bump our documentation pins, fixes an issue with a missing package (<a href="">#2011</a>)</li>
</ul>
<h2>Version 2.21.0</h2>
<ul>
<li>⚠️ Update CPython 3.12 to 3.12.6, which changes the macOS minimum deployment target on CPython 3.12 from macOS 10.9 to macOS 10.13 (<a href="">#1998</a>)</li>
<li>🛠 Changes the behaviour when inheriting <code>config-settings</code> in TOML overrides - rather than extending each key, which is rarely useful, individual keys will override previously set values. (<a href="">#1803</a>)</li>
<li>🛠 Update CPython 3.13 to 3.13.0rc2 (<a href="">#1998</a>)</li>
<li>✨ Adds support for multiarch OCI images (<a href="">#1961</a>)</li>
<li>🐛 Fixes some bugs building Linux wheels on macOS. (<a href="">#1961</a>)</li>
<li>⚠️ Changes the minimum version of Docker/Podman to Docker API version 1.43, Podman API version 3. The only mainstream runner this should affect is Travis Graviton2 runners - if so you can <a href="">upgrade your version of Docker</a>. (<a href="">#1961</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/pypa/cibuildwheel/blob/main/docs/changelog.md">pypa/cibuildwheel's changelog</a>.</em></p>
<blockquote>
<hr />
<h2>title: Changelog</h2>
<h1>Changelog</h1>
<h3>v2.22.0</h3>
<p><em>23 November 2024</em></p>
<ul>
<li>🌟 Added a new <code>CIBW_ENABLE</code>/<code>enable</code> feature that replaces <code>CIBW_FREETHREADED_SUPPORT</code>/<code>free-threaded-support</code> and <code>CIBW_PRERELEASE_PYTHONS</code> with a system that supports both. In cibuildwheel 3, this will also include a PyPy setting and the deprecated options will be removed. (<a href="">#2048</a>)</li>
<li>🌟 <a href="https://peps.python.org/pep-0735/">Dependency groups</a> are now supported for tests. Use <code>CIBW_TEST_GROUPS</code>/<code>test-groups</code> to specify groups in <code>[dependency-groups]</code> for testing. (<a href="">#2063</a>)</li>
<li>🌟 Support for the experimental Ubuntu-based ARMv7l manylinux image (<a href="">#2052</a>)</li>
<li>✨ Show a warning when cibuildwheel is run from Python 3.10 or older; cibuildwheel 3.0 will require Python 3.11 or newer as host (<a href="">#2050</a>)</li>
<li>🐛 Fix issue with stderr interfering with checking the docker version (<a href="">#2074</a>)</li>
<li>🛠 Python 3.9 is now used in <code>CIBW_BEFORE_ALL</code>/<code>before-all</code> on linux, replacing 3.8, which is now EoL (<a href="">#2043</a>)</li>
<li>🛠 Error messages for producing a pure-Python wheel are slightly more informative (<a href="">#2044</a>)</li>
<li>🛠 Better error when <code>uname -m</code> fails on ARM (<a href="">#2049</a>)</li>
<li>🛠 Better error when repair fails and docs for abi3audit on Windows (<a href="">#2058</a>)</li>
<li>🛠 Better error when <code>manylinux-interpreters ensure</code> fails (<a href="">#2066</a>)</li>
<li>🛠 Update Pyodide to 0.26.4, and adapt to the unbundled pyodide-build (now 0.29) (<a href="">#2090</a>)</li>
<li>🛠 Now cibuildwheel uses dependency-groups for development dependencies (<a href="">#2064</a>, <a href="">#2085</a>)</li>
<li>📚 Docs updates and tidy ups (<a href="">#2061</a>, <a href="">#2067</a>, <a href="">#2072</a>)</li>
</ul>
<h3>v2.21.3</h3>
<p><em>9 October 2024</em></p>
<ul>
<li>🛠 Update CPython 3.13 to 3.13.0 final release (<a href="">#2032</a>)</li>
<li>📚 Docs updates and tidy ups (<a href="">#2035</a>)</li>
</ul>
<h3>v2.21.2</h3>
<p><em>2 October 2024</em></p>
<ul>
<li>✨ Adds support for building 32-bit armv7l wheels on musllinux. On a Linux system with emulation set up, set <a href="https://cibuildwheel.pypa.io/en/stable/options/#archs">CIBW_ARCHS</a> to <code>armv7l</code> on Linux to try it out if you're interested! (<a href="">#2017</a>)</li>
<li>🐛 Fix Linux Podman builds on some systems (<a href="">#2016</a>)</li>
<li>✨ Adds official support for running on Python 3.13 (<a href="">#2026</a>)</li>
<li>🛠 Update CPython 3.13 to 3.13.0rc3 (<a href="">#2029</a>)</li>
</ul>
<p>Note: the default <a href="https://cibuildwheel.pypa.io/en/stable/options/#linux-image">manylinux image</a> is <strong>scheduled to change</strong> from <code>manylinux2014</code> to <code>manylinux_2_28</code> in a cibuildwheel release on or after <strong>6th May 2025</strong> - you can set the value now to avoid getting upgraded if you want. (<a href="">#1992</a>)</p>
<h3>v2.21.1</h3>
<p><em>16 September 2024</em></p>
<ul>
<li>🐛 Fix a bug in the Linux build, where files copied to the container would have invalid ownership permissions (<a href="">#2007</a>)</li>
<li>🐛 Fix a bug on Windows where cibuildwheel would call upon <code>uv</code> to install dependencies for versions of CPython that it does not support (<a href="">#2005</a>)</li>
<li>🐛 Fix a bug where <code>uv 0.4.10</code> would not use the right Python when testing on Linux. (<a href="">#2008</a>)</li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>ee63bf1</code></a> Bump version: v2.22.0</li>
<li><a href=""><code>d3eeba7</code></a> chore: bump Ruff to 0.8.0 (<a href="">#2092</a>)</li>
<li><a href=""><code>109020e</code></a> Updates for Pyodide builds after <code>pyodide-build</code> was unvendored (<a href="">#2090</a>)</li>
<li><a href=""><code>fd99000</code></a> [pre-commit.ci] pre-commit autoupdate (<a href="">#2087</a>)</li>
<li><a href=""><code>e158f22</code></a> ci: update gitlab for dependency-groups (<a href="">#2089</a>)</li>
<li><a href=""><code>8f21eb1</code></a> chore: use dependency-groups (<a href="">#2064</a>)</li>
<li><a href=""><code>aac31ae</code></a> docs: fix update scripts usage (<a href="">#2067</a>)</li>
<li><a href=""><code>b882b84</code></a> fix: update the macOS image used on Cirrus CI (<a href="">#2085</a>)</li>
<li><a href=""><code>243085f</code></a> [Bot] Update dependencies (<a href="">#2086</a>)</li>
<li><a href=""><code>df6f886</code></a> feat: add manylinux armv7l (<a href="">#2052</a>)</li>
<li>Additional commits viewable in <a href="https://github.com/pypa/cibuildwheel/compare/bd033a44476646b606efccdd5eed92d5ea1d77ad...ee63bf16da6cddfb925f542f2c7b59ad50e93969">compare view</a></li>
</ul>
</details>
<br />

Updates `actions/upload-artifact` from 4.3.6 to 4.6.0
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/actions/upload-artifact/releases">actions/upload-artifact's releases</a>.</em></p>
<blockquote>
<h2>v4.6.0</h2>
<h2>What's Changed</h2>
<ul>
<li>Expose env vars to control concurrency and timeout by <a href="https://github.com/yacaovsnc"><code>@​yacaovsnc</code></a> in <a href="">actions/upload-artifact#662</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/upload-artifact/compare/v4...v4.6.0">https://github.com/actions/upload-artifact/compare/v4...v4.6.0</a></p>
<h2>v4.5.0</h2>
<h2>What's Changed</h2>
<ul>
<li>fix: deprecated <code>Node.js</code> version in action by <a href="https://github.com/hamirmahal"><code>@​hamirmahal</code></a> in <a href="">actions/upload-artifact#578</a></li>
<li>Add new <code>artifact-digest</code> output by <a href="https://github.com/bdehamer"><code>@​bdehamer</code></a> in <a href="">actions/upload-artifact#656</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/hamirmahal"><code>@​hamirmahal</code></a> made their first contribution in <a href="">actions/upload-artifact#578</a></li>
<li><a href="https://github.com/bdehamer"><code>@​bdehamer</code></a> made their first contribution in <a href="">actions/upload-artifact#656</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/upload-artifact/compare/v4.4.3...v4.5.0">https://github.com/actions/upload-artifact/compare/v4.4.3...v4.5.0</a></p>
<h2>v4.4.3</h2>
<h2>What's Changed</h2>
<ul>
<li>Undo indirect dependency updates from <a href="">#627</a> by <a href="https://github.com/joshmgross"><code>@​joshmgross</code></a> in <a href="">actions/upload-artifact#632</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/upload-artifact/compare/v4.4.2...v4.4.3">https://github.com/actions/upload-artifact/compare/v4.4.2...v4.4.3</a></p>
<h2>v4.4.2</h2>
<h2>What's Changed</h2>
<ul>
<li>Bump <code>@actions/artifact</code> to 2.1.11 by <a href="https://github.com/robherley"><code>@​robherley</code></a> in <a href="">actions/upload-artifact#627</a>
<ul>
<li>Includes fix for relative symlinks not resolving properly</li>
</ul>
</li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/upload-artifact/compare/v4.4.1...v4.4.2">https://github.com/actions/upload-artifact/compare/v4.4.1...v4.4.2</a></p>
<h2>v4.4.1</h2>
<h2>What's Changed</h2>
<ul>
<li>Add a section about hidden files by <a href="https://github.com/joshmgross"><code>@​joshmgross</code></a> in <a href="">actions/upload-artifact#607</a></li>
<li>Add workflow file for publishing releases to immutable action package by <a href="https://github.com/Jcambass"><code>@​Jcambass</code></a> in <a href="">actions/upload-artifact#621</a></li>
<li>Update <code>@​actions/artifact</code> to latest version, includes symlink and timeout fixes by <a href="https://github.com/robherley"><code>@​robherley</code></a> in <a href="">actions/upload-artifact#625</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/Jcambass"><code>@​Jcambass</code></a> made their first contribution in <a href="">actions/upload-artifact#621</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/upload-artifact/compare/v4.4.0...v4.4.1">https://github.com/actions/upload-artifact/compare/v4.4.0...v4.4.1</a></p>
<h2>v4.4.0</h2>
<h2>Notice: Breaking Changes :warning:</h2>
<p>We will no longer include hidden files and folders by default in the <code>upload-artifact</code> action of this version. This reduces the risk that credentials are accidentally uploaded into artifacts. Customers who need to continue to upload these files can use a new option, <code>include-hidden-files</code>, to continue to do so.</p>
<p>See <a href="https://github.blog/changelog/2024-08-19-notice-of-upcoming-deprecations-and-breaking-changes-in-github-actions-runners/">&quot;Notice of upcoming deprecations and breaking changes in GitHub Actions runners&quot;</a> changelog and <a href="">this issue</a> for more details.</p>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>65c4c4a</code></a> Merge pull request <a href="">#662</a> from actions/yacaovsnc/add_variable_for_concurrency_a...</li>
<li><a href=""><code>0207619</code></a> move files back to satisfy licensed ci</li>
<li><a href=""><code>1ecca81</code></a> licensed cache updates</li>
<li><a href=""><code>9742269</code></a> Expose env vars to controll concurrency and timeout</li>
<li><a href=""><code>6f51ac0</code></a> Merge pull request <a href="">#656</a> from bdehamer/bdehamer/artifact-digest</li>
<li><a href=""><code>c40c16d</code></a> add new artifact-digest output</li>
<li><a href=""><code>735efb4</code></a> bump <code>@​actions/artifact</code> from 2.1.11 to 2.2.0</li>
<li><a href=""><code>184d73b</code></a> Merge pull request <a href="">#578</a> from hamirmahal/fix/deprecated-nodejs-usage-in-action</li>
<li><a href=""><code>b4a0a98</code></a> Merge branch 'main' into fix/deprecated-nodejs-usage-in-action</li>
<li><a href=""><code>b4b15b8</code></a> Merge pull request <a href="">#632</a> from actions/joshmgross/undo-dependency-changes</li>
<li>Additional commits viewable in <a href="https://github.com/actions/upload-artifact/compare/v4.3.6...65c4c4a1ddee5b72f698fdd19549f0f0fb45cf08">compare view</a></li>
</ul>
</details>
<br />

Updates `actions/setup-python` from 5.1.0 to 5.4.0
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/actions/setup-python/releases">actions/setup-python's releases</a>.</em></p>
<blockquote>
<h2>v5.4.0</h2>
<h2>What's Changed</h2>
<h3>Enhancements:</h3>
<ul>
<li>Update cache error message by <a href="https://github.com/aparnajyothi-y"><code>@​aparnajyothi-y</code></a> in <a href="">actions/setup-python#968</a></li>
<li>Enhance Workflows: Add Ubuntu-24, Remove Python 3.8  by <a href="https://github.com/priya-kinthali"><code>@​priya-kinthali</code></a> in <a href="">actions/setup-python#985</a></li>
<li>Configure Dependabot settings by <a href="https://github.com/HarithaVattikuti"><code>@​HarithaVattikuti</code></a> in <a href="">actions/setup-python#1008</a></li>
</ul>
<h3>Documentation changes:</h3>
<ul>
<li>Readme update - recommended permissions by <a href="https://github.com/benwells"><code>@​benwells</code></a> in <a href="">actions/setup-python#1009</a></li>
<li>Improve Advanced Usage examples by <a href="https://github.com/lrq3000"><code>@​lrq3000</code></a> in <a href="">actions/setup-python#645</a></li>
</ul>
<h3>Dependency updates:</h3>
<ul>
<li>Upgrade <code>undici</code> from 5.28.4 to 5.28.5 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">actions/setup-python#1012</a></li>
<li>Upgrade <code>urllib3</code> from 1.25.9 to 1.26.19 in /<strong>tests</strong>/data by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">actions/setup-python#895</a></li>
<li>Upgrade <code>actions/publish-immutable-action</code> from 0.0.3 to 0.0.4 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">actions/setup-python#1014</a></li>
<li>Upgrade <code>@actions/http-client</code> from 2.2.1 to 2.2.3 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">actions/setup-python#1020</a></li>
<li>Upgrade <code>requests</code> from 2.24.0 to 2.32.2 in /<strong>tests</strong>/data by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">actions/setup-python#1019</a></li>
<li>Upgrade <code>@actions/cache</code> to <code>^4.0.0</code> by <a href="https://github.com/priyagupta108"><code>@​priyagupta108</code></a> in <a href="">actions/setup-python#1007</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/benwells"><code>@​benwells</code></a> made their first contribution in <a href="">actions/setup-python#1009</a></li>
<li><a href="https://github.com/HarithaVattikuti"><code>@​HarithaVattikuti</code></a> made their first contribution in <a href="">actions/setup-python#1008</a></li>
<li><a href="https://github.com/lrq3000"><code>@​lrq3000</code></a> made their first contribution in <a href="">actions/setup-python#645</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/setup-python/compare/v5...v5.4.0">https://github.com/actions/setup-python/compare/v5...v5.4.0</a></p>
<h2>v5.3.0</h2>
<h2>What's Changed</h2>
<ul>
<li>Add workflow file for publishing releases to immutable action package by <a href="https://github.com/Jcambass"><code>@​Jcambass</code></a> in <a href="">actions/setup-python#941</a></li>
<li>Upgrade IA publish by <a href="https://github.com/Jcambass"><code>@​Jcambass</code></a> in <a href="">actions/setup-python#943</a></li>
</ul>
<h3>Bug Fixes:</h3>
<ul>
<li>Normalise Line Endings to Ensure Cross-Platform Consistency by <a href="https://github.com/priya-kinthali"><code>@​priya-kinthali</code></a> in <a href="">actions/setup-python#938</a></li>
<li>Revise <code>isGhes</code> logic by <a href="https://github.com/jww3"><code>@​jww3</code></a> in <a href="">actions/setup-python#963</a></li>
<li>Bump pillow from 7.2 to 10.2.0 by <a href="https://github.com/aparnajyothi-y"><code>@​aparnajyothi-y</code></a> in <a href="">actions/setup-python#956</a></li>
</ul>
<h3>Enhancements:</h3>
<ul>
<li>Enhance workflows and documentation updates by <a href="https://github.com/priya-kinthali"><code>@​priya-kinthali</code></a> in <a href="">actions/setup-python#965</a></li>
<li>Bump default versions to latest by <a href="https://github.com/jeffwidman"><code>@​jeffwidman</code></a> in <a href="">actions/setup-python#905</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/Jcambass"><code>@​Jcambass</code></a> made their first contribution in <a href="">actions/setup-python#941</a></li>
<li><a href="https://github.com/jww3"><code>@​jww3</code></a> made their first contribution in <a href="">actions/setup-python#963</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/setup-python/compare/v5...v5.3.0">https://github.com/actions/setup-python/compare/v5...v5.3.0</a></p>
<h2>v5.2.0</h2>
<h2>What's Changed</h2>
<h3>Bug fixes:</h3>
<ul>
<li>Add <code>.zip</code> extension to Windows package downloads for <code>Expand-Archive</code> Compatibility by <a href="https://github.com/priyagupta108"><code>@​priyagupta108</code></a> in <a href="">actions/setup-python#916</a>
This addresses compatibility issues on Windows self-hosted runners by ensuring that the filenames for Python and PyPy package downloads explicitly include the .zip extension, allowing the Expand-Archive command to function correctly.</li>
<li>Add arch to cache key by <a href="https://github.com/Zxilly"><code>@​Zxilly</code></a> in <a href="">actions/setup-python#896</a>
This addresses issues with caching by adding the architecture (arch) to the cache key, ensuring that cache keys are accurate to prevent conflicts.
Note: This change may break previous cache keys as they will no longer be compatible with the new format.</li>
</ul>
<h3>Documentation changes:</h3>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>4237552</code></a> Improve Advanced Usage examples (<a href="">#645</a>)</li>
<li><a href=""><code>709bfa5</code></a> Bump requests from 2.24.0 to 2.32.2 in /<strong>tests</strong>/data (<a href="">#1019</a>)</li>
<li><a href=""><code>ceb20b2</code></a> Bump <code>@​actions/http-client</code> from 2.2.1 to 2.2.3 (<a href="">#1020</a>)</li>
<li><a href=""><code>0dc2d2c</code></a> Bump actions/publish-immutable-action from 0.0.3 to 0.0.4 (<a href="">#1014</a>)</li>
<li><a href=""><code>feb9c6e</code></a> Bump urllib3 from 1.25.9 to 1.26.19 in /<strong>tests</strong>/data (<a href="">#895</a>)</li>
<li><a href=""><code>d0b4fc4</code></a> Bump undici from 5.28.4 to 5.28.5 (<a href="">#1012</a>)</li>
<li><a href=""><code>e3dfaac</code></a> Configure Dependabot settings (<a href="">#1008</a>)</li>
<li><a href=""><code>b8cf3eb</code></a> Use the new cache service: upgrade <code>@actions/cache</code> to <code>^4.0.0</code> (<a href="">#1007</a>)</li>
<li><a href=""><code>1928ae6</code></a> Update README.md (<a href="">#1009</a>)</li>
<li><a href=""><code>3fddbee</code></a> Enhance Workflows: Add Ubuntu-24, Remove Python 3.8  (<a href="">#985</a>)</li>
<li>Additional commits viewable in <a href="https://github.com/actions/setup-python/compare/82c7e631bb3cdc910f68e0081d67478d79c6982d...42375524e23c412d93fb67b49958b491fce71c38">compare view</a></li>
</ul>
</details>
<br />

Updates `glotzerlab/workflows` from 0.5.0 to 0.6.0
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>a36a971</code></a> Use mdbook 0.4.43</li>
<li><a href=""><code>cd6c090</code></a> Merge pull request <a href="">#21</a> from glotzerlab/dependabot-github_actions-trunk-action...</li>
<li><a href=""><code>fd1bc8c</code></a> Bump mamba-org/setup-micromamba in the actions-version group</li>
<li><a href=""><code>7737fdd</code></a> Demonostrate the the latest action versions.</li>
<li><a href=""><code>59660cf</code></a> Merge pull request <a href="">#19</a> from glotzerlab/dependabot-github_actions-trunk-action...</li>
<li><a href=""><code>3ad7831</code></a> Merge pull request <a href="">#20</a> from glotzerlab/dependabot-pip-dot-github-workflows-tr...</li>
<li><a href=""><code>37b33ad</code></a> Remove debug CI.</li>
<li><a href=""><code>2ba65b3</code></a> Bump build from 1.2.2 to 1.2.2.post1 in /.github/workflows</li>
<li><a href=""><code>118c2a4</code></a> Bump the actions-version group with 2 updates</li>
<li><a href=""><code>227c4e6</code></a> Update examples in README.</li>
<li>Additional commits viewable in <a href="https://github.com/glotzerlab/workflows/compare/e130dd2814c7db5635babe4427dedcbd5aab76d4...a36a97114c60241b26217ce92abbb64621ac67c7">compare view</a></li>
</ul>
</details>
<br />

Updates `pypa/gh-action-pypi-publish` from 1.9.0 to 1.12.4
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/pypa/gh-action-pypi-publish/releases">pypa/gh-action-pypi-publish's releases</a>.</em></p>
<blockquote>
<h2>v1.12.4</h2>
<!-- raw HTML omitted -->
<h2>✨ What's Changed</h2>
<p>The main theme of this patch release that the support for uploading <a href="https://packaging.python.org/en/latest/specifications/core-metadata/#license-expression">PEP 639 licensing metadata</a> to PyPI has been fixed in <a href="">#327</a>.</p>
<h2>🛠️ Internal Updates</h2>
<p>A few smaller updates include the attestation existence being checked earlier in the process now, listing all the violating files together, not just one (PR <a href="">#315</a>).
And the lock file with the software available in runtime has been re-pinned in <a href="">#329</a>.
Additionally, the CI now runs the smoke-tests against both Ubuntu 22.04 and 24.04 explicitly via da900af96347cc027433720ad4f122117645459d.</p>
<p><strong>🪞 Full Diff</strong>: <a href="https://github.com/pypa/gh-action-pypi-publish/compare/v1.12.3...v1.12.4">https://github.com/pypa/gh-action-pypi-publish/compare/v1.12.3...v1.12.4</a></p>
<p><strong>🧔‍♂️ Release Manager:</strong> <a href="https://github.com/sponsors/webknjaz"><code>@​webknjaz</code></a> <a href="https://stand-with-ukraine.pp.ua">🇺🇦</a></p>
<p><strong>🙏 Special Thanks</strong> to <a href="https://github.com/dnicolodi"><code>@​dnicolodi</code></a><a href="https://github.com/sponsors/dnicolodi">💰</a> and <a href="https://github.com/woodruffw"><code>@​woodruffw</code></a><a href="https://github.com/sponsors/woodruffw">💰</a> for releasing the license metadata support fix in Twine!</p>
<p><strong>💬 Discuss</strong> <a href="https://bsky.app/profile/webknjaz.me/post/3lghhtwey522z">on Bluesky 🦋</a>, <a href="https://mastodon.social/@webknjaz/113881243992941874">on Mastodon 🐘</a> and <a href="https://github.com/pypa/gh-action-pypi-publish/discussions/330">on GitHub</a>.</p>
<p><a href="https://github.com/sponsors/webknjaz"><img src="https://img.shields.io/badge/%40webknjaz-transparent?logo=githubsponsors&amp;logoColor=%23EA4AAA&amp;label=Sponsor&amp;color=2a313c" alt="GH Sponsors badge" /></a></p>
<h2>v1.12.3</h2>
<h2>✨ What's Improved</h2>
<p>With the updates by <a href="https://github.com/woodruffw"><code>@​woodruffw</code></a><a href="https://github.com/sponsors/woodruffw">💰</a> and <a href="https://github.com/webknjaz"><code>@​webknjaz</code></a><a href="https://github.com/sponsors/webknjaz">💰</a> via <a href="">#309</a> and <a href="">#313</a>, it is now possible to publish [distribution packages] that include [core metadata v2.4], like those built using [maturin]. This is done by bumping <code>Twine</code> to v6.0.1 and <code>pkginfo</code> to v1.12.0.</p>
<h2>📝 Docs</h2>
<p>We've made an attempt to clarify the runtime and workflow shape that are expected to be supported for calling this action in: <a href="https://github.com/marketplace/actions/pypi-publish#Non-goals">https://github.com/marketplace/actions/pypi-publish#Non-goals</a>.</p>
<blockquote>
<p>[!TIP]
Please, let us know in the <a href="https://github.com/pypa/gh-action-pypi-publish/discussions/330">release discussion</a> if anything still remains unclear.
<em>TL;DR</em> always call [<code>pypi-publish</code>] once per job; don't invoke it in reusable workflows; physically move building the dists into separate jobs having restricted permissions and storing the dists as GitHub Actions artifacts; when using self-hosted runners, make sure to still use [<code>pypi-publish</code>] on a GitHub-provided infra with <code>runs-on: ubuntu-latest</code>, while building and testing may remain self-hosted; don't perform any other actions in the publishing job; don't call [<code>pypi-publish</code>] from composite actions.</p>
</blockquote>
<h2>🛠️ Internal Updates</h2>
<p><a href="https://github.com/br3ndonland"><code>@​br3ndonland</code></a><a href="https://github.com/sponsors/br3ndonland">💰</a> improved the container image generation automation to include Git SHA in <a href="">#301</a>. And <a href="https://github.com/woodruffw"><code>@​woodruffw</code></a><a href="https://github.com/sponsors/woodruffw">💰</a> added the <code>workflow_ref</code> context to Trusted Publishing debug logging in <a href="">#305</a>, helping us diagnose misconfigurations faster. <a href="">#313</a> also extends the smoke test in the CI to check against the [maturin]-made dists. Additionally, <code>jeepney</code> and <code>secretstorage</code> transitive deps have been added to the pip constraint-based lock file, as Dependabot seems to have missed those earlier.</p>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>76f52bc</code></a> Merge pull request <a href="">#329</a> from webknjaz/maintenance/runtime-lockfile-24-02-2025</li>
<li><a href=""><code>72de13b</code></a> 📌 Mass-upgrade transitive dependency pins</li>
<li><a href=""><code>1995f2e</code></a> Merge pull request <a href="">#327</a> from webknjaz/maintenance/twine-6.1-pep639</li>
<li><a href=""><code>29f40bd</code></a> 📦 Enable metadata 2.4 support in Twine</li>
<li><a href=""><code>10df67d</code></a> 📦 Enable support for PEP 639 metadata</li>
<li><a href=""><code>e0449d2</code></a> 🧪 Integrate a unified <code>alls-green</code> GHA status</li>
<li><a href=""><code>cebc64f</code></a> 🧪 Bump setuptools in smoke test to v75.8.0</li>
<li><a href=""><code>da900af</code></a> 🧪 Run smoke tests against Ubuntu 24 and 22</li>
<li><a href=""><code>8cafb5c</code></a> 💰 Sync the funding config</li>
<li><a href=""><code>916e576</code></a> Merge pull request <a href="">#315</a> from webknjaz/refactoring/attestations-exist-bundle</li>
<li>Additional commits viewable in <a href="https://github.com/pypa/gh-action-pypi-publish/compare/ec4db0b4ddc65acdf4bff5fa45ac92d78b56bdf0...76f52bc884231f62b9a034ebfe128415bbaabdfc">compare view</a></li>
</ul>
</details>
<br />

Updates `softprops/action-gh-release` from 2.0.8 to 2.2.1
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/softprops/action-gh-release/releases">softprops/action-gh-release's releases</a>.</em></p>
<blockquote>
<h2>v2.2.1</h2>
<!-- raw HTML omitted -->
<h2>What's Changed</h2>
<h3>Bug fixes 🐛</h3>
<ul>
<li>fix: big file uploads by <a href="https://github.com/xen0n"><code>@​xen0n</code></a> in <a href="">softprops/action-gh-release#562</a></li>
</ul>
<h3>Other Changes 🔄</h3>
<ul>
<li>chore(deps): bump <code>@​types/node</code> from 22.10.1 to 22.10.2 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#559</a></li>
<li>chore(deps): bump <code>@​types/node</code> from 22.10.2 to 22.10.5 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#569</a></li>
<li>chore: update error and warning messages for not matching files in files field by <a href="https://github.com/ytimocin"><code>@​ytimocin</code></a> in <a href="">softprops/action-gh-release#568</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/ytimocin"><code>@​ytimocin</code></a> made their first contribution in <a href="">softprops/action-gh-release#568</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/softprops/action-gh-release/compare/v2.2.0...v2.2.1">https://github.com/softprops/action-gh-release/compare/v2.2.0...v2.2.1</a></p>
<h2>v2.2.0</h2>
<h2>What's Changed</h2>
<h3>Exciting New Features 🎉</h3>
<ul>
<li>feat: read the release assets asynchronously by <a href="https://github.com/xen0n"><code>@​xen0n</code></a> in <a href="">softprops/action-gh-release#552</a></li>
</ul>
<h3>Bug fixes 🐛</h3>
<ul>
<li>fix(docs): clarify the default for tag_name by <a href="https://github.com/alexeagle"><code>@​alexeagle</code></a> in <a href="">softprops/action-gh-release#544</a></li>
</ul>
<h3>Other Changes 🔄</h3>
<ul>
<li>chore(deps): bump typescript from 5.6.3 to 5.7.2 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#548</a></li>
<li>chore(deps): bump <code>@​types/node</code> from 22.9.0 to 22.9.4 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#547</a></li>
<li>chore(deps): bump cross-spawn from 7.0.3 to 7.0.6 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#545</a></li>
<li>chore(deps): bump <code>@​vercel/ncc</code> from 0.38.2 to 0.38.3 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#543</a></li>
<li>chore(deps): bump prettier from 3.3.3 to 3.4.1 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#550</a></li>
<li>chore(deps): bump <code>@​types/node</code> from 22.9.4 to 22.10.1 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#551</a></li>
<li>chore(deps): bump prettier from 3.4.1 to 3.4.2 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#554</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/alexeagle"><code>@​alexeagle</code></a> made their first contribution in <a href="">softprops/action-gh-release#544</a></li>
<li><a href="https://github.com/xen0n"><code>@​xen0n</code></a> made their first contribution in <a href="">softprops/action-gh-release#552</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/softprops/action-gh-release/compare/v2.1.0...v2.2.0">https://github.com/softprops/action-gh-release/compare/v2.1.0...v2.2.0</a></p>
<h2>v2.1.0</h2>
<!-- raw HTML omitted -->
<h2>What's Changed</h2>
<h3>Exciting New Features 🎉</h3>
<ul>
<li>feat: add support for release assets with multiple spaces within the name by <a href="https://github.com/dukhine"><code>@​dukhine</code></a> in <a href="">softprops/action-gh-release#518</a></li>
<li>feat: preserve upload order by <a href="https://github.com/richarddd"><code>@​richarddd</code></a> in <a href="">softprops/action-gh-release#500</a></li>
</ul>
<h3>Other Changes 🔄</h3>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/softprops/action-gh-release/blob/master/CHANGELOG.md">softprops/action-gh-release's changelog</a>.</em></p>
<blockquote>
<h2>2.2.1</h2>
<h2>What's Changed</h2>
<h3>Bug fixes 🐛</h3>
<ul>
<li>fix: big file uploads by <a href="https://github.com/xen0n"><code>@​xen0n</code></a> in <a href="">softprops/action-gh-release#562</a></li>
</ul>
<h3>Other Changes 🔄</h3>
<ul>
<li>chore(deps): bump <code>@​types/node</code> from 22.10.1 to 22.10.2 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#559</a></li>
<li>chore(deps): bump <code>@​types/node</code> from 22.10.2 to 22.10.5 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#569</a></li>
<li>chore: update error and warning messages for not matching files in files field by <a href="https://github.com/ytimocin"><code>@​ytimocin</code></a> in <a href="">softprops/action-gh-release#568</a></li>
</ul>
<h2>2.2.0</h2>
<h2>What's Changed</h2>
<h3>Exciting New Features 🎉</h3>
<ul>
<li>feat: read the release assets asynchronously by <a href="https://github.com/xen0n"><code>@​xen0n</code></a> in <a href="">softprops/action-gh-release#552</a></li>
</ul>
<h3>Bug fixes 🐛</h3>
<ul>
<li>fix(docs): clarify the default for tag_name by <a href="https://github.com/alexeagle"><code>@​alexeagle</code></a> in <a href="">softprops/action-gh-release#544</a></li>
</ul>
<h3>Other Changes 🔄</h3>
<ul>
<li>chore(deps): bump typescript from 5.6.3 to 5.7.2 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#548</a></li>
<li>chore(deps): bump <code>@​types/node</code> from 22.9.0 to 22.9.4 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#547</a></li>
<li>chore(deps): bump cross-spawn from 7.0.3 to 7.0.6 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#545</a></li>
<li>chore(deps): bump <code>@​vercel/ncc</code> from 0.38.2 to 0.38.3 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#543</a></li>
<li>chore(deps): bump prettier from 3.3.3 to 3.4.1 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#550</a></li>
<li>chore(deps): bump <code>@​types/node</code> from 22.9.4 to 22.10.1 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#551</a></li>
<li>chore(deps): bump prettier from 3.4.1 to 3.4.2 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#554</a></li>
</ul>
<h2>2.1.0</h2>
<h2>What's Changed</h2>
<h3>Exciting New Features 🎉</h3>
<ul>
<li>feat: add support for release assets with multiple spaces within the name by <a href="https://github.com/dukhine"><code>@​dukhine</code></a> in <a href="">softprops/action-gh-release#518</a></li>
<li>feat: preserve upload order by <a href="https://github.com/richarddd"><code>@​richarddd</code></a> in <a href="">softprops/action-gh-release#500</a></li>
</ul>
<h3>Other Changes 🔄</h3>
<ul>
<li>chore(deps): bump <code>@​types/node</code> from 22.8.2 to 22.8.7 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#539</a></li>
</ul>
<h2>2.0.9</h2>
<ul>
<li>maintenance release with updated dependencies</li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated...

_Description has been truncated_

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `15b59bd449ce`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
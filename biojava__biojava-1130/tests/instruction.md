# Issue

**Title:** Bump com.fasterxml.jackson.core:jackson-databind from 2.13.4.2 to 2.18.9 in /biojava-structure

## Description

Bumps [com.fasterxml.jackson.core:jackson-databind](https://github.com/FasterXML/jackson-databind) from 2.13.4.2 to 2.18.9.
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>f7b3266</code></a> [maven-release-plugin] prepare release jackson-databind-2.18.9</li>
<li><a href=""><code>55e3028</code></a> Prep for 2.18.9 release</li>
<li><a href=""><code>59abdc4</code></a> ...</li>
<li><a href=""><code>2fc7bd9</code></a> Do not allow DNS resolution when deserializing <code>InetAddress</code> (<a href="">#6058</a>)</li>
<li><a href=""><code>c628b35</code></a> Honor <code>@JsonView</code> for external-type-id (<code>EXTERNAL_PROPERTY</code>) properties [GHSA...</li>
<li><a href=""><code>d627a8a</code></a> Fix <a href="">#6060</a> -- <code>@JsonView</code> skipped for Setter/Field properties with `@JsonUnwra...</li>
<li><a href=""><code>bc1613c</code></a> Backport PR <a href="">#5964</a> into 2.18 to fix <a href="">#5962</a> (<a href="">#6039</a>)</li>
<li><a href=""><code>ff5d313</code></a> Add CVE id for <a href="">#5971</a></li>
<li><a href=""><code>f95549d</code></a> Update CVE id for <a href="">#5969</a></li>
<li><a href=""><code>6d6bfea</code></a> Update CVE info for <a href="">#5951</a></li>
<li>Additional commits viewable in <a href="https://github.com/FasterXML/jackson-databind/compare/jackson-databind-2.13.4.2...jackson-databind-2.18.9">compare view</a></li>
</ul>
</details>
<br />

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9cd191c4dbd3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.